memcached:
- in-memory hashtable
- set, get, and delete operations
- deployed in clusters, items are distributed through consistent hashing

reducing latency
- parallel requests and batching
	- application code needs to request large numbers of key-value pairs when loading a page
	- app constructs DAG representing data dependencies
	- batch in topological order
- UDP for get requests
	- get requests use UDP with sequence numbers to detect dropped / out of order packets
	- in those cases, memcached returns error
	- app will query the main DB but will not update the cache
		- avoid loading the possibly overloaded system
	- set and delete still use TCP for reliability
- flow control
	- use a sliding window to limit the number of concurrent, outstanding requests

reducing load
- leases
	- upon cache misses, memcached servers give lease tokens once every 10 seconds
	- If the client gets the token, it supplies it when updating memcached. If a delete request was received in between, the token is invalidated by memcached
	- If the client can't get the token, it fails and tries again in 10 seconds (the one that got the token should have added the key by then)
	- avoids _stale set_ and _thundering herd_ problems
- pools
	- favour replication over diving the key space (sharding)

handling failures
- use a designated set of machines called the Gutter
- on failure (no response) from normal server, send request to the Gutter instead
- limits load on backend services at the cost of slightly stale data
- unlike rehashing the keys, doesn't suffer from non-uniform key access frequency overloading any one server

regions
- comprise storage (mysql DB) cluster and several memcached and web servers
- cache is invalidated by mcsqueal daemon according to the mysql commit log
- multiple frontend clusters use the same _regional pool_ of memcached servers
- cold clusters can read from another 'warm' cluster to fill up faster

cross region consistency
- 1 region holds master DBs and other regions contain read-only replicas
- MySQL's replication mechanism is used
- remote markers are used to indicate if data in the local replica DB is stale

improvements
- automatically expand the hashtable
- enable multithreading
- memory is organised into slabs, each using the LRU policy. These slabs are periodically rebalanced such that their LRU entry is about the same age
- transient item cache: short lived items are put in the transient item cache and proactively evicted upon expiry while others are lazily evicted (only when queried)