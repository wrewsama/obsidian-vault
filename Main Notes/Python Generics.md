Tags:
- [[Python]]
---
- type checking with generic types in Python
- `T = TypeVar('T')`
- `TypeVar` must be assigned to a variables (`T` in this case)
- the argument to `TypeVar` must be a string equal to the name of the variable to which the type var is assigned
- type variables must not be redefined
## Example
```python
from typing import Generic, TypeVar, List

# function
T = TypeVar('T')
def foo(bar: T) -> List[T]:
    return [bar, bar, bar]
    
# class
U = TypeVar('U')
class Foo(Generic[U]):
    def __init__(self, bar: U):
        self.bar = bar
    def get(self) -> U:
        return self.bar
```

## Constraints
```python
# specific types
from typing import TypeVar
T = TypeVar('T', int, float) # must be either int or float

# subtype
U = TypeVar('U', bound=Foo) # U must be subtype of Foo
```
---
## References
- https://www.tutorialspoint.com/python/python_generics.htm
- https://typing.python.org/en/latest/spec/generics.html