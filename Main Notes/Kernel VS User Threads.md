Tags:
- [[Operating Systems]]
- [[Concurrency]]
---

|            | Kernel                                                                | User                                         |
| ---------- | --------------------------------------------------------------------- | -------------------------------------------- |
| management | OS kernel (obviously)                                                 | thread library (e.g. Green Threads)          |
| speed      | slower, requires context switch to kernel mode                        | faster, stays in user mode                   |
| blocking   | if 1 thread blocks, kernel can schedule another thread to run instead | if 1 thread blocks, whole process is blocked |

---
## References
- https://www.baeldung.com/cs/user-thread-vs-kernel-threads
- https://stackoverflow.com/questions/40877998/why-blocking-system-calls-blocks-entire-procedure-with-user-level-threads