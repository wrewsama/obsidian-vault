Tags:
- [[Python]]
- [[Software Engineering]]
---
- It's a good practice to have a case for every possible value when switch-casing an enum
- When we add a new value to the enum, we want to immediately know if there's somewhere where we're not handling the new value
- In Python, we can use the [[Python Never Type|Never]] type

example:
```python
from typing import Never
from enum import Enum

class MyEnum(Enum):
    X = 0
    Y = 1
    # uncommenting the line below will cause an error
    # Z = 2 
    
def assert_unreachable(arg: Never):
    pass

def foo(bar: MyEnum):
    match bar:
        case MyEnum.X:
            pass
        case MyEnum.Y:
            pass
        case _:
            # essentially, we don't want anything reaching here
            assert_unreachable(bar) 
```
---
## References
- https://www.linkedin.com/feed/update/urn:li:activity:7310300428095680512/