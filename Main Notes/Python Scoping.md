Tags:
- [[Python]]
---
## scopes
- local: inside function
- enclosing: enclosing function (if any)
- global
- built-in: python's builtin names (e.g. `print`, `len`)

## rule
- for reading, order of checking: LEGB
- for modifying
    - by default, will try to modify local. If nonexistent, will initialise new variable with that name
    - use `nonlocal` and `global` to refer to enclosing and global scopes
```python
x = 1
def foo():
    global x
    x = 2
foo()
print(x) # 2
```

## misc
- classes don't count as "enclosing" their methods, hence `nonlocal` won't let you modify class variables. Access using `ClassName.variable` instead
- only function blocks create scopes, not `for`, `if/else`, `try/except`, etc.
    - different from other languages!
- list comprehensions create their own scope
```python
x = 100
print([x for x in range(3)]) # [0, 1, 2]
print(x) # 100
```
---
## References
- https://www.tutorialspoint.com/What-are-the-basic-scoping-rules-for-python-variables
- https://stackoverflow.com/questions/291978/short-description-of-the-scoping-rules/23471004#23471004
- https://blog.dailydoseofds.com/p/how-a-for-loop-and-list-comprehension