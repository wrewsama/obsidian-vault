Tags:
- [[Python]]
---
- the `__new__` dunder method in Python is the "real" constructor for objects
- `__init__` _initialises_ the fields in the instance, after `__new__` creates it

```python
class Foo:
    def __new__(cls, arg):
        print(f"Creating with argument: {arg}")
        return super().__new__(cls)

    def __init__(self, arg):
        print(f"Initializing with argument: {arg}")
        super().__init__()
# Creating instance with argument: 5
# Initializing instance with argument: 5
Foo(5)
```

---
## References
- https://medium.com/@zackbunch/when-to-use-new-in-python-58632249b9cc