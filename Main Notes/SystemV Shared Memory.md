Tags:
- [[Operating Systems]]
- [[Linux]]
---
- Legacy alternative to [[POSIX Shared Memory]]
- implementation & usage:
    - user process calls `shmget` and kernel creates new/finds an old shared memory segment
    - user process attaches to it with `shmat`
- Disadvantages
    - no resizing - shm region size is fixed at creation
    - can't use unix syscalls for file descriptors (e.g. `epoll()`, `fstat()`)

---
## References
- 