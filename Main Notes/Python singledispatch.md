Tags:
- [[Python]]
---
## what
- another way to do overloaded functions in Python
- but only works on implementations with the same number of args but different types
    - need to use optional args + [[Python Overloading]]

## example
```python
from functools import singledispatch

@singledispatch
def formatter(arg):
    return f"Default: {arg}"

@formatter.register
def _(arg: int):
    return f"Integer: {arg + 100}"

@formatter.register
def _(arg: list):
    return f"List of length: {len(arg)}"

print(formatter("Hello"))  # Output: Default: Hello
print(formatter(10))       # Output: Integer: 110
print(formatter([1, 2]))   # Output: List of length: 2
```
---
## References
- https://docs.python.org/3/library/functools.html#functools.singledispatch