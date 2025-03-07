Tags:
- [[Operating Systems]]
- [[Concurrency]]
---

| Threads                                                                  | Processes                              |
| ------------------------------------------------------------------------ | -------------------------------------- |
| Only have separate hardware contexts, OS and memory contexts are  shared | Entire execution context is duplicated |

Pros and cons of threads over processes

Benefits
- Economy
	- less resources to manage compared to multiple processes
- Resource sharing
	- No need additional mechanism to pass info around since there are shared resources
- Responsiveness
	- can appear more responsive
- Scalability
	- multithreaded programs can utilise multiple CPUs (though processes can too)

Problems
- Syscall concurrency
- possibility of parallel syscalls and re-entrancy problems on global variables
- Process Behaviour
	- Process operations get affected, no isolation
	- e.g. if 1 thread does exit() or exec() or fork(), what happens to other threads and the process

---
## References
- 