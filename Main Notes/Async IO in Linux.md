Tags:
- [[Operating Systems]]
---
**O_NONBLOCK**
* Set using `fcntl` on a file descriptor
* reading: if no data is available, return immediately with err instead of blocking
* writing: if there's no space available in the write buffer, return immediately with err instead of blocking

**io_uring**
* contains 2 ring buffers
	* submission queue
	* completion queue
	- these are in a region of memory shared between user and kernel space => zero copy
- IO requests (e.g. read/write) are sent to the submission queue
	- several requests can be submitted at once => less context switches & syscalls => better performance
- once done, they'll be put into the completion queue


**poll vs select**
- poll has no fd limit
- poll only checks the fds in the `pollfd` array while select has to scan all fds up to the max value

**epoll vs poll**
- epoll is only on linux
- poll needs to scan through all it's monitored fds (O(N))
- epoll maintains the list of fds in the kernel => O(1)
- thanks to the better performance, epoll is better for large numbers of simultaneous connections (e.g. Redis server connections)

---
## References
- 