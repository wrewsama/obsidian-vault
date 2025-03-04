## Chapter 1: MySQL Architecture
```
	      Clients
          │ │ │ │
 ┌────────────────────────────┐
 │ Connection/thread handling│
 ├────────────────────────────┤
 │      │              │      │
 │      v              v      │
 │ ┌──────────┐   ┌────────┐  │
 │ │Query     │<──│ Parser │  │
 │ │Cache     │   └────────┘  │
 │ └──────────┘        │      │
 │                     │      │
 │                     v      │
 ├────────────────────────────┤
 │        Optimizer           │
 └────────────────────────────┘
          │ │ │ │ │
  ┌───┬───┬───┬───┬───┐
  │ S │ S │ S │ S │ S │   (Storage engines)
  └───┴───┴───┴───┴───┘
```

layer 1: connection management:
- each connection gets its own thread within the server process
- handles authentication and security
layer 2: optimisation and execution:
- check query cache for query result and return if present
- parse queries to create parse tree, then optimise
- rewrite query, determine table read order, choosing indexes, etc.
- provides functionality used across different storage engines (e.g. stored procedures, triggers, etc.)
layer 3: storage engine
- for storing and retrieving data
- server communicates through storage engine API (set of low level functions (not sql))


Concurrency control:
- locks
	- read / write locks: shared / exclusive
	- lock granularity: table level / row level
	- 2 phase locking: can acquire locks at any time during a transaction, but only releases them on COMMIT or ROLLBACK
- isolation levels
	- read uncommitted
	- read committed
	- repeatable read
	- serialisable
- MVCC:  keep snapshots of data to allow nonlocking reads, each storage engine implements it differently

transaction logging
- on changes, storage engine changes in-memory copy instead of the disk (mem is faster), then records the change in the transaction log (sequential IO, fast)
- on crash, use transaction log to recover the changes

storage engines
- InnoDB
	- transactions, row-level locking with MVCC
- many others

tables can be moved from 1 storage engine to another via: `ALTER TABLE t ENGINE = InnoDB`

## Chapter 2: Benchmarking and Profiling
- Benchmarking: Measure system's performance, "how does this perform?"
- Profiling: Find where application spends time / consumes resources, "why does this perform this way?"

#### Benchmarking
- either full-stack or single-component
- metrics
	- transactions per unit time
	- latency
	- scalability (i.e. how it performs with changes in database size, concurrency, or hardware)
- planning
	- identify problem and goal
	- choose standard bench or design your own
		- easy way: snapshot database, then log queries seen in production
- accuracy
	- benchmark should be repeatable
	- remove external load
	- change as few parameters as possible between benchmarks
#### Profiling
- profiling app
	- create a debug mode in app code that logs all queries along with time & number of rows
- MySQL profiling
	- enable the log in MySQL with a configuration directive
	- analyse the log and look for queries that:
		- take a long time
		- constitute most of the server's overall execution time
		- weren't high-impact previously but suddenly became high impact
- OS profiling
	- finding PID of MySQL:
		- run `SHOW PROCESSLIST` to get the port number
		- run netstat on the port number to get the PID
	- run tools like `strace -p` and `gdb -p` to show syscalls and backtraces

## Chapter 3: Schema Optimisation and Indexing
#### Data types
- guidelines
	- try to store data in the smallest, simplest data type possible 
		- example: `AUTO_INCREMENT`ed integers instead of strings for identifiers
			- faster operations (especially comparisons), more sequential IO, better cache locality (since logically adjacent rows are stored close by)
	- avoid NULL if possible
- whole numbers
	- TINYINT, SMALLINT, MEDIUMINT, INT, BIGINT: 8, 16, 24, 32, 64 bits respectively, 2s complement
	- can be UNSIGNED as well
- real numbers
	- DECIMAL: exact fractional numbers
	- FLOAT, DOUBLE: 4, 8 bytes respectively
- strings
	- VARCHAR(x): takes up a variable amount of space, up to x bytes, plus 1 or 2 extra bytes to store the length of the stored value (hence taking up x+1 or x+2 bytes total)
	- CHAR(x): always allocated x bytes, but no overhead for storing length
	- TEXT/BLOB: for storing large amounts of data, stored separately from the rest of the row, can't sort by or index the full length
	- ENUM: field value is stored as an integer, keeps a centralised lookup table mapping that integer to the name
- dates / times
	- finest granularity for MySQL: 1 second
	- DATETIME: 'concatenate' the date and time values into an integer
	- TIMESTAMP: stores epoch
- bits
	- BIT: just a bit
	- SET: bitset

#### Indexing tips
- isolate the column in queries
	- i.e. `WHERE idxed_col = 5`, NOT `WHERE some_function(idxed_col) = 5` (can't use index)
- use prefix indexes on long character columns
	- selectivity: number of distinct values / number of rows
	- set the index prefix length to be the smallest value such that the selectivity is similar to indexing the whole column

#### Index types
- clustered indexes: rows are stored inside the index's leaf pages so related data is close together, only 1 per table
- covering indexes: indexes that contain all the columns needed for a query

#### Index tricks
- indexes let queries lock fewer rows (since fewer rows need to be accessed on the way to the desired data)
- optimising limit offset on non-covering indexes 
	- issue: for small limit and large offset, need to scan through all the rows just to get a few at the end
	- solution: do the limit and offset on a subquery getting only the primary from a covering index, then join the rest of the table on the primary key and select the desired columns from there
- find and repair table corruption
	- no-op alter: `ALTER TABLE t ENGINE=innodb` set the engine to the same one it's already using
- use summary tables
	- for aggregations e.g. `COUNT`, aggregate those values periodically in a separate table and query that table instead of the main one
- counters
	- use multiple rows and let each updater update a random one
	- this prevents updaters from locking each other and getting blocked
	- to get the result, sum up the multiple counters

## Chapter 4: Query Performance Optimisation
Query execution steps
- client sends SQL to server
- server checks query cache
	- if present, just return that result
- server parses, preprocesses, and optimises SQL into an execution plan
- query execution engine executes the plan by making calls to the storage engine API
- server sends result to client

Optimising queries
- basic issues
	- fetching more rows / columns than needed
		- solution: just remove useless rows (LIMIT) and columns (from the SELECT clause)
	- MySQL scanning too much data
		- use covering indexes
		- change schema (e.g. summary tables)
		- chop up complicated query into multiple simple ones
- `COUNT`
	- type 1: `COUNT(col)` counts number of rows where `col` is not null
	- type 2: `COUNT(*)` counts number of rows
		- more efficient since it doesn't need to check the value of any column
	- another optimisation: when counting a very large subset (much more than 50% of the rows), based on a well indexed `WHERE` clause, count the negation and subtract it from the total count. This will scan fewer rows
- `JOIN`
	- use indexes on columns in the `ON` clauses
	- ensure each `GROUP BY` or `ORDER BY` are on columns within the same table (can use indexes for that table)
- `LIMIT`/`OFFSET`
	- when taking a small limit with a large offset, MySQL needs to scan the entire offset just to get the small number of rows
	- do the offset on a covering index (less columns => less pages to scan same offset => faster), then join back to the original table to get the rest of the columns
- `SQL_CALC_FOUND_ROWS`
	- usually used in pagination to check if there is more data
	- inefficient because it needs to get the entire result set, then throw it away
	- better approach: `LIMIT n + 1`, only display `n` rows and use the existence of the `n+1th` to determine if there is a next page
- `UNION`
	- MySQL creates a temporary table and fills it with the `UNION` results
	- need to manually push down `WHERE`, `LIMIT`, etc. clauses to each `SELECT` in the `UNION` since the query optimiser can't

## Chapter 5: Advanced MySQL Features
- Query Cache
	- disabled by default
	- can speed up performance of expensive, common queries
	- adds overhead to cache miss reads (need to update cache) and writes (need to invalidate cache entries that use tables they changed)
	- can tune using various configs e.g. `query_cache_type` (off, on, etc.), `query_cache_size`, etc.
	- general tips
		- smaller tables => better invalidation granularity on writes
		- batch writes => invalidation only happens once
		- disable for write-heavy apps
		- decrease cache size if it's taking too long to invalidate entries
- Stored procedures / functions
	- keep it small and simple, best for very small queries
	- keep complex logic in app code
- triggers
	- executed before / after INSERT, UPDATE, or DELETE
	- can be helpful for updating denormalised tables and summary tables
- events
	- basically cronjobs
- prepared statements
	- essentially a skeleton of a query that gets parsed, processed, and stored
	- client and server use a binary protocol to send only the parameters to fill in the skeleton's placeholders
	- faster because
		- no need to re-parse and process
		- can send less data during the request
- views
	- table that doesn't actually store any data, derived from a SQL query
	- 2 algorithms, chosen by the server 
		- MERGE: server prioritises this, merges the view's SQL query with the query on the view
		- TEMPTABLE: executes the view's SQL query, writes it to a temporary table, then executes the query on the view on that temporary table
- foreign keys
	- needs indexes on the column in BOTH the referenced and referencing tables (for inserts on the referencing table, deletes on the referenced table, or updates on either table)
	- can take up a lot of space if 1 of the tables is huge

## Chapter 6: Optimising Server Settings
- how to configure:
	- configuration file @ `etc/my.cnf` or `/etc/mysql/my.cnf`
	- dynamic configuration variables: `SET var = value` / `SET GLOBAL var = value`
- memory
	- InnoDB Buffer Pool: caches indexes, row data, insert buffer, locks, and other internal structures
	- thread cache: holds threads that aren't serving a connection. Avoids overhead of destroying and recreating threads
	- InnoDB data dictionary: per-table cache for object metadata
- IO
	- transaction log buffer size
	- log buffer flushing policy
	- InnoDB tablespace: essentially InnoDB's own virtual filesystem
- Concurrency
	- `innodb_thread_concurrency`: how many threads can be in the kernel at once
	- `innodb_commit_concurrency`: how many threads can commit at once
- BLOB and TEXT workloads
	- serve can't use in-memory temporary tables
		- convert to VARCHAR using `SUBSTRING()`
		- use memory based filesystem (`tmpfs`)
		- `COMPRESS()` the BLOB
        
## Chapter 7: OS / Hardware Optimisation
- CPUs
    - MySQL has scaling issues with multiple CPUs
    - generally (but not always) having faster CPUs are more important than having more CPUs
- Memory
    - Find an effective memory-to-disk space ratio
    - Done by trying to make the workload CPU-bound (this means the cache is doing well and the app doesn't need to wait too long on IO)
- Tuning RAID
    - RAID type
    - stripe chunk size
    - RAID cache
- Network
    - Skip DNS name resolution and supply only IP addresses
- Checking OS
    - `vmstat` and `iostat`

## Chapter 8: Replication
- replication benefits
    - load balancing for read queries
    - high availability
    - cross region data distribution
- process
    - master records changes to data in binary log
        - either statement-level or row-level
        - not to be confused with the transaction log used for crash recovery, that's disk page level
    - slave pulls master's binary log events to its own relay log
    - slave executes changes in relay log
- topologies
    - single master with multiple slaves
    - master-master in active-passive mode
        - 2 master nodes, can only write to 1 of them at any one time
        - both can have their own slaves
    - master, distribution master, and slaves
        - avoid forcing the master to keep dumping its binlog to each slave, which can be costly if there are too many
        - use a distribution master in between the true master and the slaves (i.e. distribution master is a slave to the true master, the true slaves are slaves to the distribution master)
        - use the Blackhole storage engine on the distribution master to avoid making it need to actually perform the queries
    - tree
        - slaves are slaves to each other (in an acyclic way), forming a tree structure
- monitoring
    - `SHOW MASTER STATUS`
    - do periodic heartbeat updates on master with the current timestamp so that it will be replicated onto the slaves, then compare the timestamps to determine slave lag
    - `mk-table-checksum`: calculates checksum for a table, then it gets replicated, then compare to see if slaves are consistent with the master
- resyncing slave from master
    - `mysqldump`
    - `mk-table-sync`
- replication problems
    - data corruption/loss
    - nondeterministic statements e.g. `LIMIT`
    - different storage engines on master and slave
    - data changes on slave
    - non unique server IDs 
    - undefined server IDs
    - dependencies on nonreplicated data
    - missing temporary tables
    - not replicating all updates
    - writing to both masters in master-master
    - replication lag
    - oversized packets from master
    - limited replication bandwidth
    - no disk space

## Chapter 9: Scaling and High Availability
- buying time before scaling
    - optimise performance
    - switch to better hardware
- scaling up
- scaling out
    - replication, then let slaves handle reads
    - partition workload
        - functional partitioning
        - sharding
            - partitioning keys
                - partition by the unit of sharding: some important entity ID (e.g. user id)
                - might need to shard by multiple keys to might querying by those keys efficient (though this means some data needs to be duplicated)
            - fixed allocation: shard is chosen based on a function of the partition key's value
            - dynamic allocation: partition key value to shard id mapping is stored in a table
            - explicit allocation: app chooses
            - globally unique IDs
                - `auto_increment` + `auto_increment_offset`
                - `AUTO_INCREMENT` in a global node / some equivalent global cache
                - use GUIDs (e.g. UUID)
- scaling back: purging useless data
- load balancing
    - goals:
        - scalability
        - efficiency
        - availability
        - transparency (abstraction)
        - consistency (in the sense that stateful interactions should go to the same server)
- high availability
    - build in redundancy and failover/failback when needed
        - shared-storage architectures: 2 servers using the same filesystem
        - replicated disk architecture
        - synchronous / semi-synchronous replication (like write concern in MongoDB [[NoSQL Distilled]])
    - eliminate single points of failure

## Chapter 10: Application-Level Optimisation
- find and solve common problems
    - overly high CPU, disk, network, memory usage
    - app doing the database's work and vice versa
    - app querying more data than it has to
    - app making unnecessary connections to database
- web server issues
    - processes take up x amounts of memory, when reused for something simple, it still has that much memory
    - processes can be kept alive with long lived, slow connections
    - solutions:
        - caching
        - `gzip` compression
        - proxy to serve clients while minimising number of running web server processes
- optimal concurrency
- caching
    - types
        - local
        - local shared-memory
        - distributed
        - on-disk
    - invalidation policies
        - TTL
        - explicit invalidation (on writes)
            - invalidate only or update with new value
        - invalidation on read

## Chapter 11: Backup and Recovery
backup types
- logical: SQL or delimited text
    - SQL dump: `mysqldump`
        - restoration: just execute the `.sql` file outputted by the dump
    - parallel dump and restore: `mysqlpdump`, `mk-parallel-dump`
    - delimited file: `SELECT INTO OUTFILE`
        - restoration: `LOAD DATA INFILE`
- raw: the files themselves
    - backup method:
        - LVM snapshots
    - restoration:
        - shut down server
        - move files into place
        - check server config and file permissions
        - restart

things to back up:
- table data
- schema
- binary logs
- transaction logs
- code (triggers, stored procedures)
- configs (replication, server)
- OS files (e.g. cronjobs, user/group configs)

## Chapter 12: Security
- DB permissions
    - DB stores grant tables, which map users to their privileges
    - there are grant tables corresponding to:
        - global privileges
        - database level privileges
        - table level
        - column level
    - MySQL will go down the hierarchy until it finds a match granting the desired privilege, else deny access
    - add/remove/view grants with `GRANT`/`REVOKE`/`SHOW GRANTS`
- problems that cause performance issues
    - too many privileges => many grant table entries to scan through
    - privileges too fine-grained => more grant tables to check
- network security options
    - localhost-only
    - firewall + whitelist 
    - VPN
    - SSL
    - SSH tunnelling
- data encryption
    - hashing passwords: `ENCRYPT(), SHA1(), MD5()`
    - encrypted filesystem
    - app level encryption
    - encrypting/decrypting in MySQL: `AES_ENCRYPT`/`AES_DECRYPT`

## Chapter 13: MySQL Server Status
- SHOW STATUS: shows the server's status variables
- SHOW INNODB STATUS: show information about InnoDB internal state
- SHOW PROCESSLIST: currently connected _threads_(not processes lol) and their status info
- SHOW MUTEX STATUS: status of mutexes in MySQL's internal code, used to identify concurrency issues
- replication status
    - SHOW MASTER STATUS
    - SHOW BINARY LOGS (shows the file names and sizes)
    - SHOW BINLOG EVENTS (the events in the bin logs)
    
- INFORMATION_SCHEMA database
    - DB with system views that can be queried with sql
    - most views correspond to the above SHOW commands, but there are some extra views

## Chapter 14: Tools
- interfaces
    - MySQL Visual Tools
    - SQLyog
    - phpMyAdmin
- monitoring
    - Nagios
    - MONyog
    - innotop
- utilities
    - MySQL Proxy
    - Maatkit Utilities