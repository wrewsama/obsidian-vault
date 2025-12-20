Tags:
- [[Python]]
- [[Concurrency]]
---
## what
- class in the `asyncio` library (python 3.11+)
- manage groups of async tasks

## usage
```python
import asyncio

async with asyncio.TaskGroup() as tg:
    task1 = tg.create_task(coro()) 
    task2 = tg.create_task(coro()) 
    task3 = tg.create_task(coro()) 
# all tasks are waited for upon exiting the context manager scope
print(task1.result())
```

## benefits
- automatic waiting: no need to remember to `await` every task you create
- multiple exception handling
    - in a `TaskGroup`, if multiple tasks get exceptions, an `ExceptionGroup` capturing all of them is raised
    - in old alternatives like `gather()`, only the first exception is propagated

---
## References
- 