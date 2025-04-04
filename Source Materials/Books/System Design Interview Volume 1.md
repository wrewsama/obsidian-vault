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
        - allows user requests to be served by any server
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

---
Source: https://www.goodreads.com/book/show/54109255-system-design-interview-an-insider-s-guide?ac=1&from_search=true&qid=a6rdJLb4Zd&rank=1
