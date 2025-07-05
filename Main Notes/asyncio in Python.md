Tags:
- [[Python]]
---
Letting I/O tasks run in the background without blocking to wait for them to complete

- use `async def` to define a coroutine
    - when calling a coroutine function, need to use `await` to get the results
- schedule the tasks using `asyncio.gather` or `asyncio.create_task`
	- example: `asyncio.gather(f1(), f2())`
    - note that we want the **coroutine object**, not the function i.e. `f1()` not `f1`
- when running the program, use `asyncio.run` to start the event loop
- the event loop
	- pick a ready task
	- if the task encounters an`await`, it yields control back to the event loop
- event loop runs on a single thread	

```python
async def foo():
    await asyncio.sleep(5)
    print('foo')

async def bar():
    await asyncio.sleep(3)
    print('bar')

async def main():
    asyncio.gather(foo(), bar())

if __name__ == '__main__':
    asyncio.run(main())
```

other extensions:
- async generators
    - just a generator that `awaits` before getting the value to yield
    - `async for ...`
- async context manager
    - instead of `__enter__` and `__exit__`, define `__aenter__` and `__aexit__`, which can `await` for things (e.g. setting up a DB connection)
    - instead of `with`, use with `async with ...`
- async queues
    - Similar to Go Channels [[Goroutines and Channels]]
    - `asyncio.queue`, `await my_queue.put(x)`, `x = await my_queue.get()`

---
## References
- https://realpython.com/async-io-python/