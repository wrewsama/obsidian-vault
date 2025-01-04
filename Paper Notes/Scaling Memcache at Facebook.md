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

// bookmark: 3.2