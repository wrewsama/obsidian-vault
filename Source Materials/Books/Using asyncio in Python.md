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

## Coroutines
- `async def foo()` defines a _coroutine function_
- it returns a _coroutine_
- Object wrapping a function that can stop prematurely and be restarted by the caller
- This is similar to generators
    - in fact, you initiate a coroutine with `coro.send(None)`
    - and get its return value by catching `StopIteration` and reading its `value` field
    - we can `send` into the coroutine to keep starting it
    - and `throw` into the coroutine to send in an exception
        - e.g. `asyncio.CancelledError` to cancel the coroutine

## Futures and Tasks
- `Future`
    - encapsulates state of some activity
    - can
        - get and set the result
        - cancel it
        - add additional callbacks to run when the future completes
- `Task`
    - inherits from `Future`
    - encapsulates state of a coroutine
---
Source: https://www.goodreads.com/book/show/50083143-using-asyncio-in-python
