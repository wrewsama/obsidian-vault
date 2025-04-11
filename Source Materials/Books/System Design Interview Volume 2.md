Tags:
- [[System Design]]
---
## Proximity Service
- get businesses and info based on location
- geohash
    - recursively split map into 4 parts and assign a 2-bit number to each quadrant
    - the `ith` pair of bits in the geohash refers to which of the 4 quadrants at level `i` the location belongs to
    - geographically closer locations have numerically closer geohashes => we can make an index on this
- business data is stored in business DB (optional cache) and accessed through the business service
- business data is synced with geohash Redis server
- search requests are routed to the Location-Based Service, which calculates the desired geohashes and searches the geohash redis cluster for businesses

## Nearby Friends
- clients use a websocket connection with the load balancer to update location
- load balancer forwards to websocket servers
- sender's websocket servers:
    - send location to its own Redis pub/sub channel
- each user's websocket connection handler subscribes to all their friends' channels
- receiver websocket servers:
    - compute distance from sender's location
    - return to user

## Map
- navigation service
    - based on Dijkstra's, but running it on the entire world map will be inefficient
    - split into sets of routing tiles with different resolutions (e.g. 1 set with big tile / low resolution, 1 set with small tiles and high resolution)
    - run Dijkstra's on those tiles
- location service: track user's location
    - batch location updates
    - use database that supports high write volume and scalability (e.g. Cassandra)
- map rendering
    - serve map tile images from CDN
    - compress the images

## Distributed Message Queue
- Use Zookeeper to handle
    - metadata (info about the topics e.g. number of partitions)
    - state storage (consumer-partition mapping, consumer offsets)
    - coordination (rebalancing, leader election)
- ensure durability with replication
    - configurable ACK setting to balance performance and durability
- producers
    - buffer messages to send them in batches: minimise network costs
    - handles routing to an appropriate broker's partition
- consumers
    - joins a consumer group by sending a message to the group's coordinator
    - group's coordinator tells it which partition to consume from
    - fetch messages from the partition and commit the incremented offset

## Metric Monitoring
- Metric sources: API servers, DBs, message queues, etc
- Metrics collector: gathers the metrics
    - pull model: calls the metric sources (e.g. use a service discovery to get the metrics sources, then call a `GET /metrics` endpoint)
    - push model: metric sources have a collection agent that send data to the one of the collector servers through a load balancer
- Kafka: channels data from the collectors to the Time Series DB
    - prevent data loss if DB dies
- Query Service: decouples downstreams from the DB, handle the queries by querying the DB + cache
- Time series DB
    - e.g. InfluxDB, Prometheus
    - Downsample data based on age
- Alert system: query the query service and send notifications if needed
- Visualisation system
    - query the query service
    - can use a ready-made solution like Grafana

## Ad Click Event Aggregation
- requirements
    - aggregate number of clicks of certain `ad_id`s
    - return top 100 most clicked `ad_id`s
    - support aggregation filtering by different attributes
- DB: workload is write-heavy, use NoSQL/time series like Cassandra/InfluxDB
- workflow
    - log watcher sends ad logs to message queue
    - data aggregation service (MapReduce) aggregates the data and sends it to another message queue
    - database writers pull the aggregated data and save it into the DB
    - query service / dashboard queries the DB
- lambda architecture: 2 separate processing paths (batch and streaming)
- kappa architecture: only 1 processing path combining both batch and streaming
- dealing with delayed events: watermarks
- data deduplication: track offsets then increment + commit them after sending downstream

## Reservation System
- User side:
    - CDN to serve static content
    - public API gateway between client and the various services
    - microservices
        - hotel rooms
        - ratings
        - reservations
        - payments
- admin side:
    - private gateway (usually protected by VPN)
    - hotel management service
- concurrency issues
    - double booking: solve by adding an idempotency key to the reservation request. This key can be part of the reservation page
    - race conditions:
        - pessimistic locking: `SELECT ... FOR UPDATE`
        - optimistic locking: `version` column
        - database constraints (e.g. doing an atomic increment in the query and adding a nonnegative constraint on the booking count)
- consistency
    - 2PC
    - Saga: each service does a local transaction and sends a message to the next step. If an error occurs, rollback the local transaction and send a message to the previous step to undo theirs

## Distributed Email
- sending
    - client sends mail request to web servers via HTTPS
    - web servers validate the request
    - if there is an error, send to error queue
    - if destination is in the same domain as sender, save email data directly to DB + cache + object store
    - else, send to output queue
    - SMTP servers pull messages from output queue and send them to destinations through SMTP
    - save the email in the 'sent' folder of the sender
- receiving
    - SMTP server receives the mail and adds it to the incoming mail queue
    - processing servers
        - run checks (viruses, spam)
        - store mail in DB + cache + object store
    - If user online, send to real time websocket servers that push the message to the user
    - User can also use a HTTPS request on the web servers to access the received mail

## Object Storage
- API service: handles user requests
    - upload
        - A&A
        - send update to metadata service
        - upload to data service
    - download
        - A&A
        - query metadata service to get object ID
        - query data service
- metadata
    - bucket: id, name, policies, etc.
    - object: ID, name, version, bucket, etc.
- data store
    - routing service: 
        - query placement service for data node to read/write from
        - handle data transfer to/from that data node
    - placement service
        - chooses which nodes to store objects
        - handle failover
        - detect failures through heartbeats
    - data nodes (storage)
        - primary/secondary replication: better performance and deals with node failure
        - can use erasure coding for more durability against disc failure
        - checksums for error detection
---
Source: https://www.goodreads.com/book/show/60631342-system-design-interview-an-insider-s-guide?ac=1&from_search=true&qid=rdZCpXSMbI&rank=2
