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