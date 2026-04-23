Tags:
- [[Python]]
---
- `/`: everything to the left must be positional arguments
- `*`: everything to the right must be keyword arguments

```python
def foo(a, /, b, *, c):
    ...
    
foo(1, 2, 3) # fail. Last arg should be keyword
foo(a=1, b=2, c=3) # fail. First arg should be positional
foo(1, b=2, c=3) # ok
foo(1, 2, c=3) # ok
```
---
## References
- https://realpython.com/python-asterisk-and-slash-special-parameters/