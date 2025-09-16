Tags:
- [[Python]]
---
- Method Resolution Order
- for a class that may have multiple inheritance and an inheritance tree determines
    - which method is invoked (in the case of polymorphism)
    - which class `super()` refers to (the next one in the MRO)
- can be directly checked with `ClassName.mro()`
- rules (C3 Linearisation)
    - child always before parents
    - base classes in the class definition are organised left to right
---
## References
- https://codefinity.com/blog/Understanding-Method-Resolution-Order-in-Python
- https://www.geeksforgeeks.org/python/method-resolution-order-in-python-inheritance/