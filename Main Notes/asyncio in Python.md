Tags:
- [[Python]]
---
Letting I/O tasks run in the background without blocking to wait for them to complete

- use `async def` to define a coroutine
	- this can also be used to define an async generator, which you can loop through with `async for`
	- you can `async def` `__aenter__` and `__aexit__` to make a async context manager, which can be entered with `async with`
- when running the program, use `asyncio.run` to start the event loop
- schedule the tasks using `asyncio.gather` or `asyncio.create_task`
	- example: `asyncio.gather(f1(), f2())`
- the event loop
	- pick a ready task
	- if the task encounters an`await`, it yields control back to the event loop
- event loop runs on a single thread	


---
## References
- 