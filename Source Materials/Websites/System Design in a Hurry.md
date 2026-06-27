Tags:
- [[System Design]]
---
# In a Hurry
---
## Delivery Framework
- Requirements
    - Functional: "Users should be able to X"
    - Non-Functional: "The system should be Y"
- (optional) capacity estimation (only if there are relevant back-of-the-envelope estimations that would affect the design)
- core entities
- API
    - protocol
    - endpoints
- (optional) Data flow (only if there are lengthy flows of data)
- High level design
    - diagram for each API endpoint
    - diagram showing how the different components
    - focus on a simple design that covers the functional requirements
- Deep dives (might be interviewer-led)
    - adjust to meet non-functional requirements
    - address edge cases
    - in general, improving on the high-level design

## Core Concepts Overview
- Network protocols: HTTP / SSE / websockets / gRPC
- Load balancers: layer 7 vs layer 4
- API design
- Data modelling: Relational vs Non-relational, normalised vs denormalised
- Database indexing
- Caching: pattern (e.g. cache-aside), invalidation strategy, failure handling
- Sharding: choice of shard key, ensuring transactions aren't cross-shard
- Consistent hashing
- PACELC theorem
- key latency and concurrency numbers for common components

## Key Technologies Overview
- Databases
    - relational
    - non-relational
- blob storage
- search optimised database (inverted index)
- API gateway
- Load balancer
- Queue
- Streams
- Distributed Lock
- Distributed Cache
- CDN

## Common Patterns
- realtime updates
    - default: HTTP polling
    - performant: SSE
    - bidirectional: websocket
- long running tasks
    - submit to queue, save task to DB, return ok
    - workers pull from queue, do task, save to DB
- contention
    - DB level locks
        - optimistic
        - pessimistic
    - distributed locks, 2PC, or queues
- scaling reads
    - DB optimisation: indexing + denormalisation
    - read replicas
    - caching
- scaling writes
    - handle write bursts with queues
    - sharding with a good shard key
- handling large blobs
    - server return presigned URLs to CDN
    - client uploads / downloads from the CDN servers
- multi-step processes
    - single server orchestrator
    - workflow / durable execution systems
- proximity services
    - geospatial indexes to efficiently search by location

# Core Concepts
---
## Networking
- transport layer
    - TCP: more reliable (in order, no drops or errors)
    - UDP: faster
- app layer
    - REST
        - conventions for HTTP endpoints
        - **the default option**
    - GraphQL
        - lets the client query for a part of the backend's data
        - **used when frontend needs to iterate quickly relative to the backend**
    - gRPC
        - binary protocol based on a user-defined schema (IDL)
        - **internal, server-to-server, performance-sensitive, APIs**
    - SSE
        - server streams multiple messages in a single HTTP response
        - **when clients need to receive events as they happen**
    - websockets
        - persistent connection with bidirectional communication
        - **e.g. games, real-time apps**
- load balancing
    - client-side (e.g. Redis, DNS)
        - small number of clients _that we control_ (need to update when servers change)
    - layer 4
        - routes based on transport layer info - means persistent TCP connections will naturally stay pinned to the same server
        - faster
        - **good for websockets**
    - layer 7
        - routes based on the actual request content
        - **good for HTTP traffic except websockets**
- regionalisation latency
    - CDNs
    - partitioning into regions
- failures
    - retries with exponential backoff, jitter, and idempotency
    - circuit breakers (stop requests on failed servers - prevent overloading an already overloaded server resulting in cascading failures)

## API Design
- pagination
    - offset + limit in request (simplest)
    - cursor: response includes pointer to next record (for high write volume)
- versioning
    - URL (e.g `/api/v2/endpoint`) (this is the default choice)
    - HTTP header  (`Accept-Version: v2`)
- security
    - Authentication (who are you) & Authorisation (what are you allowed to do)
    - API keys: create, store in DB, verify with lookup (internal server-to-server)
    - JWT: give client token on successful login, verify token signature and read client info from it (user sessions in web/mobile apps)
- Rate limiting
    - by user, by IP, by endpoint
    - return `429 Too Many Requests`

## Data Modelling
- DB choice
    - relational
        - ACID guarantees, complex queries
        - default option
        - e.g. Postgres, MySQL
    - document
        - flexible, denormalised schema
        - e.g. MongoDB, Firestore
    - KV stores
        - simple, fast lookups
        - e.g. Redis, Memcached
    - Wide-column
        - tables have a schema, but each row may only have a subset of columns, only those values will be stored (no nulls for columns not in that row)
        - rows with the same partition key are stored together
        - good for massive append-only write volumes (e.g. timeseries)
        - e.g. Cassandra, HBase
- requirements
    - volume
    - access patterns
    - consistency
- identify core entities, their columns, then connect them with relationships - adding PKs and FKs along with any relevant constraints
- choose indexes based on access patterns
- choose normalised vs denormalised
    - default: normalised
    - denormalise only if you need to sacrifice consistency for performance/scale
- shard if the DB has insufficient space

## Database Indexing
- B Tree indexes (default option)
- LSM Trees: for high write loads
    - write to memtable (RB tree) and WAL, then return
    - if memtable reaches size limit: flush to SSTable (sorted string table)
    - asynchronously compact the SSTables
- Hash indexes: for very fast exact-matches, in practice not that much faster than B trees
- Geospatial: "nearby" queries
    - indexing latitude and longitude separately is inefficient
    - geohash: grid location -> hash value; similar hashes => close by
    - R-Tree: recursively subdivide each region into overlapping rectangles
- inverted indexes: full text search
- other optimisations
    - composite indexes
    - covering indexes (`CREATE INDEX ... ON ... INCLUDE...`)

## Caching
- architectures
    - cache aside (read cache and fallback to DB, write to DB and invalidate cache; simple default)
    - write-through (write to cache & cache writes to DB; ensure cache data always fresh)
    - write-behind / write-back (write to cache & cache writes to DB in the background; fastest, eventual consistency)
    - read-through (read cache & cache reads from DB if needed; CDNs do this)
- cache eviction
    - LRU, LFU, FIFO, TTL
- cache problems
    - thundering herd
        - popular cache entry invalidated, many requests cache miss => hit DB
        - solutions: request coalescing, preemptive refresh on popular keys
    - consistency
        - cache and DB have different values
        - solutions: cache invalidation on write(strong consistency), TTLs (bounded eventual consistency)
    - hot keys
        - solutions: replication, local cache for hot values, rate limiting
- reasons to cache
    - database load
    - latency requirements (DBs ~ 10-30ms, Redis ~ 1ms)

## Sharding
- good shard keys
    - high cardinality, even distribution, query-aligned
- strategies
    - hash-based (default)
    - range-based (simplest, but uneven distribution in many cases)
    - directory-based: save a lookup table (flexible, but worse performance)
- problems
    - hotspots
        - solutions: isolate on dedicated shards, spread out with compound shard keys, dynamic shard splitting
    - cross-shard queries
        - solutions: cache, denormalise
    - maintaining consistency
        - design to avoid cross-shard transactions, use sagas or 2PC, just accept eventual consistency
- reasons to shard
    - storage (> 64TB)
    - write throughput (> 50K)
    - read throughput (>50K) (can also use replication and caching though)

## CAP Theorem
- when to choose Consistency
    - ticket booking systems, inventory management, financial systems
- when to choose availability
    - social media, content, review sites

## Numbers To Know

| Component     | Key Metrics (single instance)                                | When to scale out                                             |
| ------------- | ------------------------------------------------------------ | ------------------------------------------------------------- |
| Cache         | latency: 1ms<br>ops/s: 100K<br>mem limit: 1TB                | hit rate < 80%<br>latency > 1ms<br>mem usage > 800GB          |
| Database      | TPS: 10k<br>read latency: 5ms<br>storage: 64TB               | TPS > 10k<br>latency > 5ms<br>need geographic distribution    |
| App server    | connections: 100k<br>cores: 8-64<br>RAM: 64-512GB, up to 2TB | CPU > 70%<br>latency > SLA<br>connections > 100k<br>mem > 80% |
| Message Queue | msgs/s: 1 million<br>latency: 5ms<br>storage: 50TB<br>       | throughput > 800k<br>partition count: 200k<br>consumer lag    |
in general, scale out when
- > 80% resource usage
- high latency
- high QPS

in general, servers have
- 10k-100k qps
- few hundred GB of memory
- tens of TB of storage

# Patterns
---
## Real Time Updates
core issue: need event (e.g. user sends chat message, LLM sends output) -> server -> client (chat recipient, LLM chatbot web interface)

- server -> user
    - simple polling: simplest
    - long polling: simple, with less latency / bandwidth usage than simple polling
        - client sends poll request to server
        - server waits until it gets and event (or timeout) then returns
        - client immediately sends next poll request
    - server-sent events: unidirectional streaming from the server to the client
        - client establishes SSE connection (e.g. with `EventSource`) (actually just a normal HTTP connection)
        - server responds with special `Transfer-Encoding: chunked` header and sends those chunks (each event) to the client
    - websockets: full-duplex client <> server
        - client and server establish HTTP connection
        - client "upgrades" the connection by initiating WebSocket handshake
        - **edge-case**: L7 load balancers don't guarantee each incoming request uses the same TCP connection, which means client requests may go to a server that doesn't have the websocket connection state, thus failing
    - webRTC: peer-to-peer
        - peers request the signalling server for peer discovery
        - connect and stream data to each other directly
        - fallback to connecting to an intermediate server (STUN or TURN) if the peers can't connect to each other (e.g. because of NAT)
- event -> server
    - pulling by polling: simplest 
        - source event (e.g. LLM, other client) updates DB
        - server exposes polling endpoint that checks DB for the latest events
    - pushing with consistent hashing: better latency, but very complex
        - event source pushes event to any server instance
        - server uses consistent hashing to determine the client to push to
    - pushing by message queue: **default option**. Good balance of performance and simplicity
        - when server establishes a connection to a client, register a topic on the message queue for that connection and subscribes to it
        - event source pushes event to message queue on the relevant topic
        - server consumes the message from the topic and sends the relevant data to the corresponding client

- deep dives
    - connection failures: use heartbeats to detect
    - celebrity problem: batching and hierarchical aggregation
    - message ordering: single node, or vector clocks

## Dealing with Contention
core issue: race conditions between different client requests

- conditional writes: simplest, best option if possible
    - single atomic `UPDATE table SET ... WHERE <condition>`
    - need to model the resource under contention as its own row
    - condition must also be simple enough to put in an SQL WHERE clause
- pessimistic locking: simple and flexible
    - `SELECT ... FOR UPDATE` to lock the rows throughout the transaction
    - you can then use application code to determine next steps
- optimistic locking: good if conflicts are rare
    - keep a monotonically increasing value column, usually a `version` number
    - `UPDATE` only if the row's `version` equals what was read at the start of the transaction
- isolation levels: for trickier problems like write skew, where there's no single row collision
    - use `SERIALIZABLE`
- distributed locks: when the resource must be held for longer than the transaction (several minutes)
    - column on DB
    - Redis value (with possible TTL)
    - ZooKeeper / etcd

- deep dives
    - deadlock prevention: ordered locking
    - ABA problem: use monotonically increasing column
    - celebrity problem: queue-based serialisation (queue + single async worker)

## Multi-Step Processes
core issue: long-running, multi-step operations; Need to handle failures and retries

- event-driven choreography: simple, fault-tolerant, and scalable. But difficult to debug as no single service knows the full saga
    - for each request, server sends event to an event store
    - each step has its own workers, that listen and write relevant events to the event store (e.g. Payment workers look for `PaymentCreated` and emit `PaymentCharged` once done)
- durable execution (e.g. Temporal): supports _reliable_ long-running processes
    - activity: an idempotent unit of work
    - workflow: deterministic orchestration of activities
    - requests go to a Temporal server, that tracks the states/results of workflows and activities in a DB
    - Workflow workers run the workflow, telling the Temporal Server to queue Activities as needed
    - Activity workers run the activities and report the results back to the Temporal Server
    - on a workflow crash, Temporal will replay the history, using the saved Activity results instead of rerunning them

- deep dives
    - the process running the saga crashes: durable progress: track what's been done and on failure, rerun the saga while skipping already-done tasks
    - handling updates: workflow versioning
    - avoiding unbounded workflow state size: continue-as-new
        - snapshot the current state and hand off to a new workflow run with an empty history
        - no need to replay the entire history
## Scaling Reads
- indexing
- vertical scaling
- denormalisation (avoid overhead of joins, at the expense of duplicated data and complexity of keeping the DB in a consistent state)
- materialised views computed via background jobs
- read replicas
    - can be sync or async, trading consistency for latency (PACELC)
- sharding
    - by domain (e.g. different tables in different database nodes)
    - geographic
- app level caching (e.g. cache-aside with Redis)
- CDN caching

- deep dives
    - latency increasing with dataset size and CPU at 100%: use indexes to avoid full-table scans
    - celebrity problem on cache: duplicate into multiple keys and client-side load balance; OR request coalescing within each backend host
    - cache stampede (hot key expires, many requests trigger DB reads for that key): early refresh - either probabilistic on each request or scheduled as a background job
    - immediate cache invalidation: cache versioning
        - server caches a version key
        - data is read using the requested key + the current version key
        - writes increment that key
        - _directly invalidating the cache is not feasible as the data could be cached on many levels (app, CDN, browser, etc.)_
## Scaling Writes
- vertical scaling
- database choice (e.g. Cassandra)
- horizontal sharding with appropriate shard key
- vertical sharding
- write queues (for burst handling)
- load shedding (also for burst handling)
    - drop writes in the backend service based on domain rules (e.g. Uber driver location updates that were already recently updated)
- batching writes
    - batching in the client
    - intermediate "batcher" service between client and core backend service
- hierarchical aggregation: process data in horizontally-partitioned stages, reducing volume at each step
    - writes (client-side) load balanced between write processor servers that send the writes to the core backend service
    - core service can load balance between broadcast nodes to send updates to consumers
    - reduces the sheer volume of incoming and outgoing connections, and allows the writes to be batched, decreasing the load on the core backend service

- deep dives
    - resharding: dual writes to both old and new shard
    - hot key: split them into multiple keys

## Handling Large Blobs
core issue: uploading/downloading files > 10MB

- direct upload / download
    - server returns a **presigned URL** to client
    - presigned URL essentially is an endpoint to the object storage (blob storage like S3 or CDN like CloudFront) with an expiry time and a signature from the server
    - client uploads/downloads directly from the object store with that URL
- resumable uploads
    - handled by cloud providers by using chunks and a a single resumable upload URL
- state synchronisation (e.g. when is the file fully uploaded): use event notifications from the object store

- deep dives
    - validating uploads: upload to quarantine bucket, run validation pipeline before promoting to prod bucket
    - metadata: server creates metadata row in DB, updates based on event notifications from the object store
    - download performance: CDNs for geographical latency; Parallel chunk downloads for large files
## Managing Long-Running Tasks
- intuition
    - client submits request to server
    - server 
        - creates job entry in DB
        - publishes job id to job queue
        - immediately returns ok to user
    - workers
        - consume from job queue
        - perform task
        - update DB
- deep dives
    - worker host failures: heartbeats
    - job failures: retries with threshold + dead letter queue
    - duplicate work: idempotency
        - key can be something like (user_id, action, rounded_timestamp) so if we round to the nearest minute, 2 duplicate requests in the same minute will be deduplicated
    - backpressure: queue depth limits
    - mixed workloads: separate queues and/or workers
        - job type or expected duration
# Key Technologies
---
## Redis
- configurations: single node, High-Availability replica, cluster
    - for clusters: nodes gossip, client asks a node for key -> node mappings, client caches it so it can query only the node with the desired data
- requests only for single nodes
- performance
    - 100k writes / second
    - < 1ms latency
- example uses: cache, distributed lock (atomic increment and delete), rate-limiting (sorted set)
## Elasticsearch
- High-performance distributed search engine
    - numeric range queries (BKD trees)
    - geospatial
    - full-text search (inverted index)
- architecture
    - master node: add / remove nodes and "tables" (called indexes in Elasticsearch)
    - data node: stores data
    - ingest node: transforms data and prepares it for indexing
    - coordinating node: optimises and routes queries to correct cluster
- data nodes
    - store the Elasticsearch index shards as Lucene indexes (tables)
    - each is split into **immutable** Lucene segments
        - writes get batched and written into a new segment
        - segments are merged in the background
- when to use
    - read-heavy on large data set (>100k documents), acceptable eventual consistency
    - attach to a true DB via Change Data Capture
## Kafka
- basic terminology
    - brokers: Kafka servers
    - partition: logical sequence of messages stored on a broker, can have multiple partitions on 1 broker
    - topic: logical grouping of partitions
- producing
    - client queries metadata from brokers
    - uses partitioning algorithm to find the partition to write to
    - send message to the correct broker
    - message is appended to the partition and assigned an offset
- consuming
    - pull from their assigned partition and current offset
    - commit the offset back to kafka
- durability
    - leader-follower replication for each partition (only for backups, don't serve traffic)
    - Kafka controller (one of the brokers) monitors health and reassigns leadership if a leader goes down
- handling hot partitions
    - no key (distributes across partitions by default), compound key, backpressure
- fault tolerance when consumer goes down
    - offsets are committed
    - partitions are rebalanced among consumers in the group
- retries
    - producer: automatic in Kafka
    - consumer: need to set up Dead Letter Queue on the app side
- optimisations
    - batching, compression
- retention policies: by max time or max size, default = 7 days
- when to use kafka
    - as a queue: async / in-order / decoupled
    - as a stream: continuous processing of real-time data
## API Gateway
- Single entrypoint for clients, abstracting away a microservice architecture
- process
    - request validation
    - middleware (auth, rate limiting, CORS etc.)
    - routing rules (e.g. based on URL pattern, request headers, etc.)
    - transformation (e.g. from HTTP to gRPC and back to HTTP)
    - optional caching
- can be horizontally scaled with a client-to-gateway load balancer (e.g NGINX) in front
- can itself load balance for the backend server instances

## Cassandra
- wide column database
- primary key contains:
    - partition key: rows are grouped and stored together based on this
    - clustering key: optional, determines sorted order of the rows
- partitions with Consistent Hashing
- information shared via gossip protocol
- queries can go to any node, which becomes the coordinator, querying the correct nodes
- tunable consistency settings for reads and writes
    - ONE, ALL, QUORUM (`n//2 + 1`)
- storage
    - commit log (WAL)
    - memtable
    - SSTable
- when to use
    - need high availability
    - high write throughput
    - clear access patterns without complex queries e.g. joins and aggregations

## DynamoDB
- managed (by AWS), scalable, KV/document store
- can think of it as documents (called items) with a _primary key_ that comprises
    - partition key (same partition key => stored together)
    - sort key (items with same partition key are sorted by sort key)
- find node by hashing partition key (consistent hashing), find item by B tree index on sort key
- secondary indexes
    - global (GSI): different partition key + some sort key, stored on a different partition, replicated with eventual consistency
    - local (LSI): same partition key + different sort key, sorted on the same partition (together with the primary sort key's B tree)
        - note: can only be defined at table creation time
- consistency is configurable for each read operation, default = eventual
- when NOT to use it
    - very high volume workloads (=> high AWS costs)
    - complex queries (joins and aggregations)

## PostgreSQL
- GIN indexes: Generalised Inverted Indexes, for simple full-text search
    - performant, but lacks elasticsearch features like relevancy scoring, fuzzy matching, distributed search, etc.
- PostGIS GiST indexes: Geospatial search index using R Trees
- write process
    - write to memory pages in buffer cache and write to WAL (sequential on disk)
    - delayed flush from memory pages to disk
        - when memory pressure gets too high or
        - checkpoint occurs
    - the above is done for the actual data as well as any indexes on the target table

## Flink
TODO

## ZooKeeper
TODO

## Time Series Databases
- e.g. Prometheus, InfluxDB
- data model
    - metric: the "table" (some value that changes over time)
    - tags: metadata for filtering (e.g. `host=xyz`, `region=us-east-1`)
    - fields: the values of the metric at each point in time
    - timestamps
- e.g. unique combination of metric + tags creates a new series, which is stored together
    - append-only to memtable + LSM tree
- maintain in-memory tag index, mapping tag => series that tag is part of (similar concept to inverted index)
- since metric values are similar across timestamps, can use
    - delta / delta-of-delta encoding
    - XOR compression (for floats)
- partition by time (e.g. 1 partition per day)
---
Source: https://www.hellointerview.com/learn/system-design/in-a-hurry/introduction
