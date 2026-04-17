Tags:
- [[Python]]
---
## what
- _class_ dunder attribute to explicitly state the attributes of every _instance_
- set up **member descriptors** containing offsets for those attributes
    - so the pointer to`instance.foo` will be at `start_address_of_instance + foo_offset`
- this is opposed to the normal flow, which is to store the object pointers in the instance's `__dict__`

## example
```python
class Foo:
    __slots__ = ("bar", "baz")
    def __init__(self, bar, baz):
       self.bar = bar
       self.baz = baz 
```
## why
- speedup (1 less dictionary lookup)
- memory savings (no dictionary overhead)

## problem
- no dynamic assignment after instantiation
    - `AttributeError` when trying to assign to a new field not defined in `__slots__`
    - but can just include `"__dict__"` as part of the `__slots__`
- complexities with inheritance
    - if parent has `__slots__`, child needs to explicitly define it in order to avoid creating a `__dict__` (but it will inherit the fields listed in the parent's `__slots__)
    - for multiple inheritance, only 1 parent can have a nonempty `__slots__`
---
## References
- https://stackoverflow.com/questions/472000/usage-of-slots
- https://medium.com/@stephenjayakar/a-quick-dive-into-pythons-slots-72cdc2d334e
- https://wiki.python.org/moin/UsingSlots