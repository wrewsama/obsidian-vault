Tags:
- [[Python]]
---
## what
- python builtin function that returns a dictionary of
    - keys: names of the symbols in the current scope
        - e.g. classes, functions, variables, etc.
    - values:
        - the values of those variables
- if called in the global scope, behaves exactly like [[Python Globals Function]]
- else, will return a fresh copy of a dict containing the symbol-value mappings
    - i.e. it cannot be used to change the values of the symbols

## examples
```python
# locals() in global scope - can modify
x = 1
locals()["x"] = 2
print(x) # 2

# locals() in local scope - cannot modify
def foo():
    y = 1
    locals()["y"] = 2 # does nothing
    print(y) # 1
foo()
```
---
## References
- https://www.geeksforgeeks.org/python/python-locals-function/