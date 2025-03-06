Tags:
- [[Python]]
---
Global Interpreter Lock: mutex in the CPython interpreter that allows only 1 thread to execute bytecode

**Purpose**
- Needed because GC uses reference counting
- Prevents race conditions if 2 threads access the same object
- Fine-grained locking for every single object incurs large overhead and could potentially lead to deadlocks

**Implications**
- multithreading doesn't help CPU-bound tasks (though IO-bound tasks can still benefit since they aren't executing bytecode while waiting, so other threads can run instead)

**Solution: Multiprocessing**
- creates multiple OS processes
- each process has its own instance of the interpreter
- can now execute CPU bound tasks at the same time (on different cores)
- higher overhead

---
## References
- 