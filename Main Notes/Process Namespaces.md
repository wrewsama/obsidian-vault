Tags:
- [[Operating Systems]]
- [[Linux]]
---
- provides an isolated set of PIDs that cannot be accessed from or gain access to the outside world
- similar to how logical memory vs physical memory works
- has its own `init` process with PID 1
    - can be created with `clone()` with `CLONE_NEWPID`
    - if this terminates, all processes in that namespace get SIGKILLed
- can use `nsenter` to run a process inside another process's namespace
- subprocesses will be in the same namespace as their parent process

---
## References
- 