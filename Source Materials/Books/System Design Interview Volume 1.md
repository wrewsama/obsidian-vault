Tags:
- [[System Design]]
---
## Scale From 0 To Millions of Users
- Data Tier
    - Relational vs non-relational: Just pick relational unless you need:
        - super low latency
        - massive amounts of data
        - only unstructured data
    - Replication (master-slave)
        - better read performance (replicas can be read in parallel)
        - higher availability
    - Cache
        - good for frequent reads and infrequent writes
        - improves performance and reduces database load
    - Deploy to multiple data centres (together with web tier)
    - Sharding: split up databases based on a shard key
        - need shard key to evenly distribute data
        - may need denormalisation to avoid expensive joins
        - may need to reshard data if a shard gets too big or if it gets too much traffic (hotspot)
- Web Tier
    - Keep web tier stateless
        - store session data in persistent storage, not in the server
        - allows us to scale out and let user requests be served by any server
        - use a load balancer to route requests between multiple servers
    - Deploy to multiple data centres (together with data tier)
        - servers within a data centre mainly access the data tier within the same data centre (lower latency)
        - use geoDNS to route users to the closest data centre 
    - Split into smaller services
        - allows for independent scaling
    - Others
        - logging
        - metrics (e.g. host level stats like CPU usage, aggregated performance metrics like database access times, or key business metrics)
        - automation (e.g. CI)
- User
    - deliver static content from a CDN

## Back-of-The-Envelope Estimation
- Latency numbers every programmer should know
```
L1 cache reference                           0.5 ns
Branch mispredict                            5   ns
L2 cache reference                           7   ns                          14x L1 cache
Mutex lock/unlock                           25   ns
Main memory reference                      100   ns                           20x L2 cache, 200x L1 cache
Compress 1K bytes with Zippy             3,000   ns        3 us
Send 1K bytes over 1 Gbps network       10,000   ns       10 us
Read 4K randomly from SSD*             150,000   ns      150 us          ~1GB/sec SSD
Read 1 MB sequentially from memory     250,000   ns      250 us
Round trip within same datacenter      500,000   ns      500 us
Read 1 MB sequentially from SSD*     1,000,000   ns    1,000 us    1 ms  ~1GB/sec SSD, 4X memory
Disk seek                           10,000,000   ns   10,000 us   10 ms      20x datacenter roundtrip
Read 1 MB sequentially from disk    20,000,000   ns   20,000 us   20 ms      80x memory, 20X SSD
Send packet CA->Netherlands->CA    150,000,000   ns  150,000 us  150 ms
```
- common estimations
    - QPS
    - peak QPS
    - storage size
    - number of servers
- tips
    - write down assumptions
    - label units
    - round and approximate

## Answering Framework
1. Understand problem and establish design scope (3-10 min)
    - features to build
    - tech stack
    - any existing services that can be used
    - expected scale
2. Propose high-level design and get buy-in (10-15 min)
    - back-of-the-envelope calculations
    - come up with an initial blueprint with box diagrams
    - treat interviewer like a teammate. Get feedback and work together to solve the problem
3. Design Deep Dive (10-25 min)
    - Work with the interviewer, dive into parts of the design
    - e.g. system performance, bottlenecks, details of system components
4. Wrap Up (3-5 min)
    - Recap your design
    - Answer any follow-up questions from the interviewer
        - operation issues
        - handling the next scale curve
        - other refinements

## Design a Rate Limiter
- requirements:
    - accurately limit requests
    - low latency
    - minimise memory usage
    - fault tolerance
    - clearly show users the exception when throttling requests
    - can be shared across multiple servers (i.e. client A making requests to server X counts towards A's limit for all servers)
- Algorithms
    - token bucket
    - leaky bucket
    - fixed window counter
    - sliding window log / counter
- Implementation
    - Rate limiting middleware between client and API servers
    - Redis with `INCR` & `EXPIRE`
    - Persist rate limiting rules and cache them
- Exceeding rate limit: 
    - Drop message with HTTP response 429
        - `X-Ratelimit-Remaining` header
        - `X-Ratelimit-Limit` header
        - `X-Ratelimit-Retry-After` header
    - OR, send to message queue
- Avoid race conditions: Lua script or sorted set

## Consistent Hashing
- want to allocate $k$ pieces of data to $n$ nodes/slots
- issue with naïve hashing + modulo: If the number of slots changes, most of the data needs to be moved to a new slot
- with consistent hashing: only $k/n$ keys need to be moved
- how it works
    - nodes are hashed with multiple functions and the outputs (known as virtual nodes) are placed on a ring
    - keys are hashed to the same ring and stored in the first node in the clockwise direction
    - this invariant is maintained when nodes are added and removed

## Design Key Value Store
- partition with consistent hashing
- strong consistency: quorum with W + R > N
- eventual consistency: versioning with a vector clock to resolve inconsistencies
- failure
    - detection: gossip protocol
        - each node maintains membership list
        - each node sends heartbeats to a set of random nodes which then forward it to other nodes
        - if a heartbeat has not been received for more than some limit, that node is considered failed
    - handling temporary failures: sloppy quorum + hinted handoff
        - let another server process requests for the down server
        - when that server comes back up, push changes back to it
    - handling permanent failures: Anti-entropy
        - maintain a Merkle tree on each replica
        - if inconsistencies are detected, update each replica to the latest version
- write path
    - persist request in commit log
    - write to memory cache
    - when memory cache is fill to a certain point, flush to SSTables on disk
- read path
    - check memory cache
    - check if key doesn't exist using bloom filter
    - otherwise, check SSTables

## Unique ID Generator
- Multi-master
    - $k$ `auto_increment` databases, each at its own offset, incrementing by $k$ each time
    - bad scaling
    - doesn't go up with time across multiple servers (results in bad cache locality)
- UUID
    - doesn't go up with time across multiple servers (results in bad cache locality)
- centralised ticket server
    - SPOF
- Snowflake ID
    - 64 bit signed integer, but the sign bit (1st bit) is always 0
    - next 41 bits: timestamp (milliseconds since some epoch: 41 bits lasts around 69 years)
    - 10 bits: machine id
    - 12 bits: sequence number

## URL Shortener
- shorten URLS
    - `POST api/v1/data/shorten`
    - persist the shortURL-longURL mappings
    - given a long URL, apply a hash function to it to obtain a candidate short url
    - check if that short url already exists
        - can optimise with bloom filter
    - if it exists, append some predefined string to the long url and hash it again. Repeat until we get an unused short url 
- redirect from shortened URL
    - `GET api/v1/:shortUrl`
    - respond with status code 301/302 and the `location` of the full link in headers
        - 301: permanently moved
        - 302: temporarily moved
        - for 301, browser caches the response so subsequent requests go straight to the long url. Lower server load but no analytics

## Web Crawler
workflow:
- URL frontier: add seed URLs
    - ensure _politeness_: from the same host, only download 1 page at a time
    - set up a queue for each worker thread
    - ensure all requests to a particular host goes to the same queue
- HTML Downloader: fetch URLs from URL frontier
    - need to check `robots.txt` to see what we're allowed to download
    - scale out to multiple servers and locations for performance and locality
- DNS resolver: get IP addresses of those URLs
- HTML Downloader: downloads pages from the IP addresses
- Content Parser: parse HTML and check if pages are malformed
- Content Seen: check if page is already in storage, if yes, discard
    - compare hashes
- Link extractor: extract links from the HTML
- URL filter: filter out certain content types, file extensions, blacklisted sites, etc.
- URL seen: check if URL is already in storage, if yes, discard, if not, add to URL frontier
- repeat

## Notification System
- when users sign in on a device, save the user-device mapping, as well as their notification settings in a cache + DB
- notification server: use the cache + DB to retrieve the data, then send a request to the appropriate message queue (e.g. android queue, iOS queue, SMS, etc.)
- each message queue is consumed by some worker servers 
    - notification template DB
    - notification log DB: track requests and message status, allows for retry mechanisms
    - downstream 3rd party notification delivery (e.g. FCM for android)
- extensions
    - rate limiting
    - analytics

## News Feed System
- feed publishing `POST /api/v1/me/feed`
    - Create post in post service (write to post cache + DB) and get a post id
    - Get friend/follower user IDs from graph DB
    - Get user settings from user cache + DB (e.g. muted, blocked)
    - hybrid fanout:
        - Fanout on write: For each user ID that isn't popular (doesn't have "too many" followers/friends), add `(post id, user id)` to the news feed cache
        - Fanout on read: for popular users, don't push the post to the news feed cache (avoid having too many writes)
- feed retrieval `GET /api/v1/me/feed`
    - fetch news feed post ids from cache
    - use post ids to get newsfeed posts from post cache + DB
    - fetch posts from followed popular users from post cache + DB
    - return the fully hydrated news feed to the client for rendering

## Chat System
login and service discovery:
- user logs in
- API servers authenticate 
- service discovery finds best chat server
- establish websocket connection

message flow:
- send message request to chat server
- source chat server sends message to message sync queue (if group message, send to all members)
- message is saved in the message store
- if receiver is online, find which server they're connected to and push the message there
- if offline, send a push notification

## Search Autocomplete
- Trie DB and cache to efficiently retrieve possible results for each prefix
    - Shard based on historical data distribution of prefixes - even distribution of load
- Data gathering service: 
    - Aggregators to ingest and aggregate search analytics logs
    - Workers to use the aggregated data to update the trie DB
- Optimisations
    - use AJAX requests to prevent refreshing the whole web page
    - enable browser caching since search autocomplete results are unlikely to change much
## YouTube
- video uploading
    - user uploads video to storage
    - transcoding servers transcode the original video
        - different formats and qualities
    - concurrently:
        - transcoded videos distributed to CDN
        - completion handlers update metadata DB + cache
- video streaming
    - client streams from CDN

---
Source: https://www.goodreads.com/book/show/54109255-system-design-interview-an-insider-s-guide?ac=1&from_search=true&qid=a6rdJLb4Zd&rank=1
