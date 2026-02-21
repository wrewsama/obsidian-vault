Tags:
- [[Python]]
---
## what
- builtin python function that returns a dictionary of:
    - keys: names of the symbols in the global scope
        - e.g. classes, functions, variables, etc.
        - includes some builtin names like `__name__`
    - values:
        - the values of those variables

## usage
- just call `globals()`
- can also use it to modify variables outside its scope [[Python Scoping]]
```python
x = 1
def foo():
    globals()["x"] = 2
foo()
print(x) # 2
```
---
## References
- https://www.geeksforgeeks.org/python/python-globals-function/