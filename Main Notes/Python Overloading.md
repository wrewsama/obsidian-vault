Tags:
- [[Python]]
---
## what
- decorator to type-annotate overloaded functions
- only for type checking; **NOT actually used to define multiple overloaded implementations of a function**

## example
```python
from typing import overload
@overload
def process(response: int) -> tuple[int, str]:
    ...
@overload
def process(response: bytes) -> str:
    ...
def process(response: int | bytes) -> tuple[int, str] | str:
    if isinstance(response, int):
        return (response, "lol")
    elif isinstance(response, bytes) :
        return response.decode()
    raise
print(process(420)) # (420, lol)
print(process(b"miko")) # miko
```

## alternative
- [[Python singledispatch]]

---
## References
- https://docs.python.org/3/library/typing.html#typing.overload