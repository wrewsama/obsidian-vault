Tags:
- [[Tags/Go|Go]]
---
Goroutines
- functions executing concurrently in the same address space
- multiplexed onto multiple OS threads by the go scheduler
	- when go program is started, runtime inits n OS threads where n = num physical cores * num threads that can run on each core (i.e max num of OS threads)
	- each OS thread has a Local Run Queue of goroutines
	- goroutines are first sent to the Global Run Queue, scheduler periodically scans GRQ and dispatches goroutines to LRQs
	- OS  thread can steal from other LRQs or GRQ once its own LRQ is empty
	
advantages of goroutines:
- smaller mem footprint
- goroutines have a growable stack space that starts at 2kb, as opposed to OS threads fixed size stack space of 2mb
- faster creation, destruction, context switches

Channels
- allows for communication between goroutines
- also provides synchronisation between reads and writes


---
## References
- 