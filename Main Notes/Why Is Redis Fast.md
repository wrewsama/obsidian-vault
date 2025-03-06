Tags:
- [[Tags/Databases|Databases]] 
- [[Redis]]
---
- in-memory database => better read/write throughput & latency than DBs that persist on disk
- Single-threaded
	- no synchronisation or context-switching overhead
	- IO multiplexing + event loop
		- 1 thread waits on many socket connections
		- uses `epoll` (more performant syscall than `poll`, handles thousands of concurrent connections)
- in memory => can implement efficient data structures without worrying about persisting them to disk
	- skip lists
	- hash tables

---
## References
- 