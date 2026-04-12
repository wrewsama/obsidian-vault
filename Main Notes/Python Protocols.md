Tags:
- [[Python]]
---
## what
- a way to perform [Duck Typing](https://stackoverflow.com/a/40434829) in Python
    - just check if the object has the required methods & fields
- inheriting from ABCs forms and IS-A relationship, Protocols provide a way to make CAN-DO relationships, without needing subclasses to explicitly inherit from anything

## example
```python
from typing import Protocol

class Fooable(Protocol):
    def foo(x: int, y: str) -> str:
        pass

# note that Bar and Baz don't even need to reference Fooable
class Bar:
    def foo(x: int, y: str) -> str:
        return str(x) + y
        
class Baz:
    def foo(x: int, y: str) -> str:
        return y + str(x)
        
def do_foo(fooer: Fooable):
    print(fooer.foo(35, "miko"))

# do_foo can take in either Bar and Baz
# because they CAN DO foo(int, str) -> str
do_foo(Bar()) # 35miko
do_foo(Baz()) # miko35
```
---
## References
- https://realpython.com/python-protocol/