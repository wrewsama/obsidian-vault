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
 │       │             │      │
 │       v             v      │
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