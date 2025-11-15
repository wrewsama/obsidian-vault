Tags:
- [[Python]]
---
## what
- `contextlib` class that lets you manage multiple [[Context Managers in Python]] in a single `with` block
- use `enter_context` to stack up context managers to "attach" to the exit stack
- use `callback` to add arbitrary functions that will be executed once the `ExitStack`'s `with` block exits (usually cleanup functions)

## example with context managers
```python
with ExitStack() as stack:
    files = [stack.enter_context(open(fname)) for fname in filenames]
    # all files will be closed once this with block exits
```

## example with arbitrary callbacks
```python
with ExitStack() as cm:
    res1 = acquire_resource_one()
    cm.callback(release_resource, res1)
    res2 = acquire_resource_two()
    cm.callback(release_resource, res2)
    
    # release resource will be called on both res1 and res2
    # once the with block exits
```
---
## References
- https://docs.python.org/3/library/contextlib.html#contextlib.ExitStack
- https://www.rath.org/on-the-beauty-of-pythons-exitstack.html