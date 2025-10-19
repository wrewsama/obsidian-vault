Tags:
- [[Python]]
- [[Concurrency]]
---

## The point of asyncio
- Handle IO bound workloads
    - without threading-related bugs e.g. some race conditions
        - can't "context switch" without an explicit `await`
        - even with the GIL, threads can have race conditions when multiple lines of bytecode are involved (e.g. incrementing a counter)
    - handle massive numbers of simultaneous connections (> thousands) without massive overhead
- it does NOT
    - make code faster than threading
        - the effect of fewer context switches is negligible unless you have several thousand concurrent tasks
    - prevent all race conditions
---
Source: https://www.goodreads.com/book/show/50083143-using-asyncio-in-python
