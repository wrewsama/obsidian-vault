Tags:
- [[Python]]
---
- part of the `typing` module 
- used for [[Python Generics]]
- essentially a `TypeVar` that captures all the parameters in a `Callable` (i.e. the `args` and `kwargs`
- `TypeVars` are unsuitable as each can only capture one parameter
- very useful in decorators, where you need to preserve type info from the original function

**example**
```python
from typing import Callable, TypeVar, ParamSpec

T = TypeVar("T")
P = ParamSpec("P")

def decorator(f: Callable[P, T]) -> Callable[P, T]:
    def wrapper(*args: P.args, **kwargs: P.kwargs) -> T:
        return f(*args, **kwargs)
    return wrapper
```
---
## References
- https://peps.python.org/pep-0612/