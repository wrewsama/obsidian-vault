Tags:
- [[Python]]
---
- Method Resolution Order
- for a class that may have multiple inheritance and an inheritance tree determines
    - which method is invoked (in the case of polymorphism)
    - which class `super()` ([[Python super]]) refers to (the next one in the MRO)
- can be directly checked with `ClassName.mro()`
- rules (C3 Linearisation)
    - Linearisation of class X `L(X)` = `merge` all `L(P)` for all of X's parents `P` in left-to-right order
    - `merge` intuition: check each the linearisation lists and keep greedily picking the 1st `valid` value
    - `valid` intuition: does not appear at a later point in any other linearisation list

## example
```python
class A:
    def foo(self):
        print("A")

class B(A):
    pass

class C(A):
    def foo(self):
        print("C")

class D(B, C):
    pass


D().foo() # C
print([cls.__name__ for cls in D.mro()]) # ['D', 'B', 'C', 'A', 'object']
```
- `L(C) = C,A,o`
- `L(B) = B,A,o`
- `L(D) = D + merge(C,A,o ; B,A,o)`
    - `= D,C + merge(A,o ; B,A,o) # note A is not valid`
    - `= D,C,B + merge(A,o ; A,o) # now A is valid`
    - `= D,C,B,A + merge(o ; o)`
    - `= D,C,B,A,o`
---
## References
- https://codefinity.com/blog/Understanding-Method-Resolution-Order-in-Python
- https://stackoverflow.com/questions/54197122/how-does-c3-algorithm-for-mro-in-python-work