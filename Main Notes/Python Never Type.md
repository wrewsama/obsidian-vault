Tags:
- [[Python]]
---
- Indicates an argument or return variable that should _Never_ have a value

Never return:
```python
from typing import Never

def fail() -> Never:
    return 123 # fails because it returns something

def also_fail() -> Never:
    pass # implicitly returns None, similar to functions that don't call return

def succeed() -> Never:
    raise Exception('hehe') # truly returns nothing
```

Never argument: often used to assert that something is not reachable
```python
from typing import Never

def assert_unreachable(arg: Never):
    pass

def fail() -> None:
    assert_unreachable(1) # errors, int is not Never

def ok() -> None:
    if False:
        assert_unreachable(1) # ok since we can never reach this
```

Note that this is all static analysis, the functions in question don't even need to be called anywhere for the errors to appear during static analysis.

---
## References
- https://docs.python.org/3/library/typing.html#typing.Never