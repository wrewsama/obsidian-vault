Tags:
- [[Distributed Systems]]
---
## Chapter 1
distributed system: ensemble of physical machines that communicate over network links

distributed system challenges:
- communication
- coordination
- scalability
- resiliency
- operations
	- testing, deployment, maintenance(e.g. monitoring)

## Chapter 2
- TCP 3-way handshake => 1 RTT for every new connection
- Flow control
	- receiver sends remaining size of receive buffer 
	- prevents overwhelming receiver
- congestion control 
	- maintain _congestion window_: number of segments that can be sent without waiting for ACK
	- congestion window increases over time, decreases when timeout is encountered
	- prevents overwhelming the network

## Chapter 3
TLS: Transport Layer Security
- encryption: negotiate shared encryption secret using asymmetric encryption (either directly (e.g. Diffie-Hellman) or but encrypting the secret using a public key (e.g. RSA))
- authentication: prove server's identity by
	- signing with private key
	- providing public key as part of a certificate signed by a CA
- integrity: verify against a hash (aka the HMAC)

parties need to agree on:
- key exchange algo for shared secret
- symmetric encryption algo
- signature algo for cert signing
- HMAC algo for integrity check

## Chapter 4
DNS: Domain Name System
1. browser
2. dns resolver (usually server hosted by ISP)
3. root name server
4. TLD
5. iteratively query the authoritative name server for each domain / subdomain in the hostname

- caches are used at every step
- every DNS record has a TTL
- DNS can easily become a single point of failure

## Chapter 5
IDL: interface definition language
OpenAPI: DIL for RESTful APIs based on HTTP

API evolution:
- REST APIs should be versioned
	- url prefix
	- custom header
	- `Accept` header
- changes should be backwards-compatible
- breaking changes should be a new version

## Chapter 6
System model: encodes assumptions about behaviour of nodes, links and timing

links:
- fair-loss: messages may be lost and duplicated. Sender can just retry and it will eventually work
- reliable link: messages are delivered exactly once
- authenticated reliable link: same as reliable, but receiver is also able to authenticate the sender

node failures:
- arbitrary-fault: nodes can deviate from their algo in arbitrary ways
- crash-recovery: nodes don't deviate from their algo, but can crash and restart at any time
- crash-stop: nodes don't deviate from their algo, but can crash and never come back

timing:
- synchronous: each action (sending message / executing operation) never takes over a certain amount of time
- asynchronous: actions can take an unbounded amount of time
- partially synchronous: behaves synchronously most of the time, but occasionally behaves asynchronously

## Chapter 7
failure detection methods:
- ping: request to check if receiver is still available - "are you alive?"
- heartbeat: request that indicates the sender is still available - "I'm still alive"
- timeout on ping ACK / no received heartbeat in given timeframe => we assume that process is dead

## Chapter 8
Physical clocks
- wall-clock time 
- done with a quartz clock
	- susceptible to clock drift / skew
	- need to sync with NTP (network time protocol), still only an estimate

logical clocks
- measure passing of time in terms of logical ops
	- Lamport clock
		- process increments local logical clock before executing op
		- process includes current timestamp when sending message
		- upon receiving, process's counter is set to 1 + max(current timestamp, message's timestamp)
		- A happens-before B => A's timestamp < B's timestamp
	- Vector clock
		- each process has a counter for every process in the system
		- before op, process increments counter corresponding to it
		- include the vector of counters in sent message
		- upon receiving, merge the vectors by taking `max` element-wise, then increment counter corresponding to it
		- if every counter in T1 <= corresponding counter in T2 && exists some counter in T1 < corresponding in T2
			- then O1 happened before O2
## Chapter 9
Raft leader election
- master-slave architecture
- election term is represented by a logical clock
- slaves receive heartbeats from master
- when timeout reached without heartbeat, slave becomes a candidate, votes for itself, and requests other processes to vote for it
- if majority vote for that candidate, it becomes the new leader
- if candidate receives heartbeat from process claiming to be the leader with a term >= that of the candidate's term, accept that leader
- if timeout is reached with no leader, start a new election process

## Chapter 10
State machine replication
1. each node has a log. A log is an order list of entries with:
	1. operation
	2. index
	3. term number
2. when leader wants to apply op, append new log entry
3. send `AppendEntries` request to followers with the new entry and the one before it
4. when followers receive the request
	1. if follower has an entry matching the prev one in the request: 
		1. ACK message
		2. leader waits for a majority of followers to ACK before returning
		3. if no majority, leader retries
	2. if request entry is already in the follower's log, ignore
	3. if prev entry in request doesn't match latest entry in follower's log:
		1. return err
		2. leader retries by sending the same entries, plus the one before those
		3. repeat until ACK

PACELC theorem: extension of CAP: during network Partition, need to tradeoff between Availability and Consistency . Else, tradeoff between Latency and Consistency

## Chapter 11
Isolation with concurrency control
- Pessimistic
	- 2PL: only acquire locks in phase 1, then only release locks in phase 2
	- better for write-heavy
- Optimistic
	- MVCC: reads can read version of data that was committed before transaction/query started, aren't blocked by other operations
	- better for read-heavy

Atomicity with 2PC
- 1 coordinator, many participants
- coordinator sends prepare request, participants respond.
	- If all participants ack, coordinator sends commit message and participants commit and report done
	- if a participant replies that it's unable to commit, coordinator sends abort message and participants abort and report done
- if coordinator dies in between prepare and commit, participants are blocked
- if participant dies after the commit message, coordinator is blocked and needs to retry
- is a synchronous, blocking protocol, usually combined with a blocking concurrency control mechanism like 2PL for isolation

Atomic async transactions
- log based transactions
	- write to message queue
	- downstreams read from the queue and update their state
- Sagas
	- when the result of 1 downstream can cause a rollback of another => cannot use log-based transaction
	- use a coordinator service that tracks the state of the transaction and based on that state, sends commands (e.g. do something, abort something) to the relevant downstream
- isolation: can be done with semantic locking
	- mark the data currently being modified by a saga with a dirty flag
	- clear the dirty flag once the saga is done
	- fail when trying to modify data with another saga's dirty flag

## Chapter 12

Microservice benefits
- less cross-team comms
- smaller codebases
- strong boundaries between services
- independent scaling
- independent data modelling and tech stack

Microservice costs
- Overhead of remote calls
- Difficult to test integration of microservices with one another 
- Strong consistency is difficult and expensive to implement, likely need to accept eventual consistency

Should start with monolith, then split into microservices only when needed.

API gateway
- facade for the client to access the internal microservices
- provides
	- routing: call appropriate backend services, client doesn't need to know details
	- composition: call multiple services in 1 API, client only needs 1 entry point
	- translation: e.g. from HTTP (internal) to gRPC (external)
	- cross-cutting concerns: e.g. caching, rate-limiting, authentication & authorisation, avoid having to re-implement in each service
- auth implementations
	- gateway receives credentials and validates it (with an auth service), then creates a security token (e.g. JWT) which is passed to downstream services
	- can also use API key to identify and restrict the upstream applications
- querying distributed data may be inefficient, might require CQRS
	- writes go through the write path, then get replicated to the read datastore
	- complex reads go to the read datastore

Messaging
- no such thing as exactly-once message deliver, can only simulate exactly-once message processing by requiring messages to be idempotent
- on failure, message consumption is retried a finite number of times, after which the message is sent to a dead letter queue
- blobs may be too large to send through a queue. Can store the blob in an object store, then send the url through the message queue instead

## Chapter 13
Sharding strategies
- range partitioning
	- issue: access patterns where a certain range of keys are accessed more frequently (e.g. dates) cause hotspots
- hash partitioning
	- issue: access patterns with singular hot keys
	- reshuffling when number of partitions changes is expensive
		- can be solved with consistent hashing

rebalancing
- static: number of partitions decided at the start, usually more than required. Each node can serve multiple partitions. Partitions are rebalanced across the nodes
- dynamic: partitions split and merge based on their size 

## Chapter 14

load balancer
- LB has 1 or more NICs each mapped to a virtual IP address which is mapped to a pool of servers
- load balancing algos: round robin, random, load metrics
- service discovery: let LB discover the available servers
- health checks: let LB know if a server is no longer available

DNS load balancing
- add IPs of all the servers into the DNS record
- let client pick
- issue: cannot respond to failures automatically, also DNS caching means changes cannot be applied immediately
- Geo load balancing: consider location of the client (inferred from IP), then returns closed IPs

Transport (layer 4) load balancing
- clients send messages to the LB
- LB changes
	- src: client => LB
	- dest: LB => the chosen server

App (layer 7) load balancing
- LB receives HTTP request from client, inspects it, then sends it to a server
- can de-multiplex HTTP requests on the same TCP connection
- can access the app level info and do things
	- rate limiting based on HTTP headers
	- terminate TLS connections
	- force HTTP requests for the same logical session to be routed to the same backend server

replication:
- single leader: e.g. Raft
- multi-leader
- leaderless: need W + R > N

caching:
- high speed storage layer that buffers responses from downstreams so that future requests can be served directly from it
- policies if item not in cache
	- side: client fetches data from the dependency and updates the cache
	- inline: cache updates itself
- in-process cache
- out-of-process cache

## Chapter 15
cruel math: when an operation has some probability of failing, the total number of failures increases with the total number of operations performed

common failure causes:
- single point of failure
- unreliable network
- slow processes
	- memory leak => less memory => OS needs to swap mem pages to disk => slow performance
	- threads getting blocked on synchronous calls => thread unavailable
	- connection in socket pool gets blocked => conn unusable
- unexpected load
- cascading failures
	- e.g. 1 replica goes down => increased load on other replica => that one also goes down

risk management: either reduce probability of failure, or reduce the impact of the failure

## Chapter 16
ensuring resiliency against downstream failures
- timeouts
	- absence of timeouts can leak to resource leaks
	- set timeouts based on desired false timeout rate: e.g. 1% false timeout rate => timeout should be set at 99th percentile response time
- retries
	- exponential backoff: exponentially increasing delay avoids adding load to an already overwhelmed system
	- random jitter: avoid retry storms
	- retry amplification
		- if upstream and downstream both have `n` retries, the downstream's downstream could face `n^2` total retries for 1 client request
		- long dependency chains should only have retries at 1 level and fail fast on the rest
	- when to NOT retry
		- non short lived error (e.g. bad request)
		- non idempotent
- circuit breaks
	- detect non transient failures and prevent upstreams from making requests to the affected downstreams altogether
	- 3 states
		- closed: normal operation, counts failures in a particular time interval
		- open: when failures / time >= threshold, blocks all requests to the downstream
		- half-open: after being open for some time, switch to half-open which checks if the next call succeeds or fails, if success, go to closed, else, go back to open

## Chapter 17
ensuring resiliency against upstream failures
- load shedding
	- each server keeps a counter of in-progress requests, reject any excess with a 503
- load levelling
	- use pull-based messaging channel between clients and service, allow service to process requests at a smoothed-out, consistent rate
- rate limiting
	- similar to load shedding, but looks at the global state instead of 1 server's local state
	- keep buckets for timeslots and increment the corresponding bucket when receiving a request at time `t`
	- use a sliding window to do a weighted sum on the buckets (based on the overlap amount) to estimate the number of requests during that period
	- reject requests with a 429 if too many requests
- bulkhead
	- prevent fault in 1 part of the service from breaking the whole thing
	- use the load balancer to assign users to a subset of instances
	- heavy / poisonous user can only affect that subset
	- other users can still use the other, unaffected instances
	- ofc if another user shares the same subset of instances as the heavy user, they will be completely blocked but this has a low probability with many instances and a decent-sized subset
- health endpoint
	- let load balancer do health checks on processes
	- if process fails, no requests will be routed to it
- watchdog
	- each process has a separate background "watchdog" thread that wakes up periodically and checks the health (e.g. memory usage)
	- if health check fails, watchdog restarts the process

## Chapter 18
tests
- unit 
- integration
- end to end

test doubles
- fake: lightweight implementation that behaves similarly to the real one
- stub: regardless of input, always returns same value
- mock: expects some input, returns a predefined output

## Chapter 19
CD stages
- review
- build
- pre-production rollout
	- deploy built artifact to pre-production env
	- services released to the pre-production env should call the production endpoints of external dependencies
- production rollout
	- canary > incremental rollout in a low-traffic region > incremental rollouts in other regions

Rollbacks
- can be triggered by 
	- e2e test results
	- health metrics (latency, errors, etc.)
	- alerts
	- health endpoints
- CD pipeline waits for a while between steps (bake time) to detect errors, if detected, can automatically rollback

breaking change into backward-compatible steps
1. prepare: make consumer accept both versions
2. activate: switch producer to new version
3. cleanup: remove old version from consumer (once confident that 2 won't be rolled back)

## Chapter 20
metrics
- numeric representation of information measured over a time interval
- services emit metrics in the form of events
- these events are pre-aggregated based on the time period and persisted in a datastore for event logs

Service Level Indicators (SLIs)
- measure the level of service
- eg
	- response time
	- availability
	- quality: proportion of requests served in an un-degraded state
	- data completeness: proportion of records that can be successfully accessed later
- should use the metric that best represents the experience of the user

long tail latencies: the right side of the resp time distributions (99, 99.9th percentiles)

Service Level Objectives (SLOs)
- range of acceptable values of SLIs for system to be considered healthy
- SLOs define an error budget, which can be used for prioritising repair tasks

Dashboards best practices
- most important at the top
- use a default timezone (e.g. UTC)
- all charts should use the same time resolutions and ranges
- minimise number of data points and metrics on each chart
- each chart should only contain metrics with similar ranges

## Chapter 21
Observability
- set of tools that provide granular insight into a prod system
- essentially monitoring, but also with tools to understand and debug it

Log
- immutable list of timestamped events
- usually some key-value pairs
- data about a specific _work unit_(e.g. 1 request) should be stored in 1 event
	- need to pass around context object containing event being built

traces
- capture the entire lifespan of a request as it propagates throughout the system
- trace id assigned at the beginning of a new request, that id is propagated from 1 stage to another