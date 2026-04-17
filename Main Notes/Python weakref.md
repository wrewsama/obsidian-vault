Tags:
- [[Python]]
---
## what
- way to access objects without increasing their reference count
- this means that objects can be garbage collected [[Python Garbage Collection]] even when the weak ref still exists
- useful for caches and observers (e.g. an object shouldn't be kept if it's only in the cache)

## example
```python
import weakref
class MyObj: ...
obj = MyObj()
ref = weakref.ref(obj)
obj.foo = "bar"
print(ref().foo) # bar (NOTE: we need to CALL ref)
```

> Note: weakrefs need the `__weakref__` dunder. So primitive data types (object, list, int, etc.), as well as objects with `__slots__` that don't contain `"__weakref__"` don't support it
---
## References
- https://medium.com/data-science/a-guide-to-pythons-weak-references-using-weakref-module-d3381b01db99