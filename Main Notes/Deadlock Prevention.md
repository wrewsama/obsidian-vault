Tags:
- [[Concurrency]]
- [[Operating Systems]]
---
- One of the [[Deadlock Handling]] methods
- Ensure at least 1 of the 4 deadlock conditions cannot hold
- Mutual exclusion
    - use lock-free data structures (e.g. by using instructions like CAS)
- hold and wait
    - acquire all locks at the same time, atomically
- no preemption
    - use a `try_lock` routine that returns an error if it can't acquire a lock, then release other locks and retry from the beginning (technically doesn't _add_ preemption, but achieves a similar effect)
    - or just enable preemption - i.e. force a lower priority thread/process to give up its locks
- circular wait
    - partial/total ordering on all locks

---
## References
- [[Operating Systems Three Easy Pieces]]