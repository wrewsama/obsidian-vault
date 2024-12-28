## Chapter 1

Reasons why NoSQL is used
- Impedance mismatch of relational DBs
	- need to translate from the data structures in the app code to the relational model in the DB
- Shift away from a shared, **integration database** to an **application database** for each app
	- applications communicate with HTTP instead of the database
	- clients no longer need to care about how the server persists the data
- Need to support large volumes of data on clusters
	- most relational DBs don't support this out of the box (e.g. postgres needs `pg_shard`)

Characteristics:
- non relational
- runs well on clusters
- schema-less

Result: Polyglot Persistence: Picking different data stores for different circumstances

## Chapter 2
Aggregate: collection of data that we interact with as a unit
Aggregate-oriented
- KV, doc, column-family
- best when most data interaction is done with the same aggregate
- atomic only within a single aggregate. Ops that update multiple aggregates may fail halfway
Aggregate-ignorant:
- relational, graph
- best when data can be organised in many different formations
	- e.g. only need 1 small field in the aggregate, or need fields across multiple aggregates

## Chapter 3
- Graph DBs: for small records with complex relationships / interactions
- The myth of schemaless: Applications usually need to assume certain field names are present and carry data with a particular meaning. (implicit schema)
- Materialised views can be used to pre-compute data in a format that's different from the way it is stored
	- can be done eagerly or by using batch cronjobs in the background

## Chapter 4
- data distribution
	- handle more data
	- handle more traffic
	- is more complex
- sharding
	- put different part of data onto different servers
	- aggregates do well because data that's accessed together will be on the same server
	- NoSQL often offers **autosharding**: DB allocates data to shards and ensures data goes to the right shard
- replication
	- master-slave
		- all writes to master
		- those changes are propagated to slaves
		- reads can be done from either
		- improves read scalability but not writes
	- p2p
		- all nodes can handle reads and writes
		- nodes communicate changes to each other

## Chapter 5
- concurrency control
	- pessimistic: prevent conflicts
	- optimistic: detect and fix conflicts
- logical consistency
	- ensuring different data items make sense together (maintain invariants)
	- affected by lost updates and dirty reads
- replication consistency
	- ensuring same data item has same value when read from different replicas
	- strong vs eventual
- session consistency
	- can read your own writes
- CAP theorem: if you get a network partition, need to trade off availability to get stronger consistency (or vice versa)
- can trade durability for latency
- can use a quorum to preserve strong consistency when replication is used
	- need `R + W > N`
	- can trade off R and W based on which operation has stricter performance requirements

## Chapter 6

Single master optimistic locking:
- version stamp + CAS

Leaderless optimistic locking:
- GUID
	- large and can't be compared directly for recentness (usually; I think mongoDB ids can)
- hash of the content
	- same issues as GUID
- timestamp
	- clock drift becomes risky
- vector stamp
	- each node stores a version number corresponding to each neighbour node

## Chapter 7
- map: data -> kv pairs
- reduce: values associated to some key -> output
- combinable reducer: reducer where output matches input => can combine into pipelines => improved parallelism, reduced network traffic

## Chapter 8
benefits of KV:
- always uses primary-key access => great performance, easy to scale

when not to use KV
- need relationships between data 
- need ops on multiple keys at once
- need to query by data (not keys)

## Chapter 9
Document: self-describing, hierarchical tree structure consisting of maps, collections, and scalars

- scaling, consistency, and availability are achieved with replica sets
- mongoDB's `slaveOk` allows client to read from slaves. Can be set for the whole client or for individual ops
- write concern can be configured to allow for consistency. Can be set for the collection or for individual ops
- by default, only single-document transactions are atomic, cannot do multiple operations in a 'transaction' (like RDBMS)

## Chapter 10
column family database:
- each row has a row key and several columns
- column
	- a name-value pair with a timestamp
	- or a map of columns
- keyspaces: collections of column families, similar to databases in RDBMS

Writing to Cassandra:
- write to commit log
- write to in-memory memtable
- periodically written out to SSTables
- SSTables are compacted

Hinted handoff: data that's supposed to be stored by a node is handed off to other nodes while it's dead

## Chapter 11
Graph DB:
- nodes are entities with some properties
- edges connect 2 nodes by some _relationship type_ and they also have their own properties
- edges are directed
- ACID-compliant
- scaled using master-slave replication
- sharding is difficult (trying to traverse across network is expensive)
- need to shard on the application side using domain knowledge (e.g. splitting by geographical location)

features:
- create node
- set node property
- create relationship (edge with the given relationship type) between 2 nodes
- create index on some property and add nodes to it
- get node and its relationships
- traverse graph (either BFS or DFS) starting from some node with a given direction and edge type
- get all paths / shortest path between 2 nodes

## Chapter 12
- RDBMS schema migrations for greenfield projects
	- store schema changes / data migration in script files
	- name the script files sequentially
	- can use 3rd party framework to run those SQL scripts in sequence to apply all new changes to the DB
- RDBMS schema migrations for legacy projects
	- create a baseline script, then do the same script files
	- during transition phase, also need to ensure backward compatibility
		- use triggers / views / virtual columns
- NoSQL schema migrations
	- implicit schema still needs backward compatibility
	- use incremental migration (on the app side)
		- reads handle both schema versions
		- writes use the new schema
		- updates do both of the above
- graph DB migrations
	- cannot just change edge types
	- traverse and add new properties / edges
	- migrate app code
	- clean up unused things
	
## Chapter 13
- Using a single DB engine for all requirements usually leads to non-performant solutions
- polyglot datastores: use different DB engines for different purposes
- service usage over direct data store usage: wrap the datastores in their own services

## Chapter 14
other persistence solutions beyond NoSQL
- file systems
- event sourcing
	- instead of persisting the state, persist the _changes_ to the state
	- broadcast those changes to downstream applications
	- each application uses a combination of a snapshot and that _event log_ to build its own state 
	- CQRS can be used
- memory image
- version control
	- provide historic queries and multiple views of the world
- XML DBs
- object DBs

## Chapter 15
- reasons to use NoSQL
	- better matches app needs => programmer productivity
	- improve data access performance
- to evaluate performance, must test under your use cases
- when uncertain of which database to use, make the database code in the app easy to replace
	- Repository pattern (within the app code)
	- Service over Direct Data Store