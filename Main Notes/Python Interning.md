Tags:
- [[Python]]
---
- memory-saving technique where commonly-used immutable objects are reused
- e.g. 
    - integers between -5 and 256
    - empty tuples (NOT lists and sets, obviously)
    - strings (size <= 4096)
```python
>>> a = 256
>>> b = 256
>>> a is b
True
>>> c = 257
>>> d = 257
>>> c is d
False
>>> s = "a"*4096
>>> t = "a"*4096
>>> s is t
True
>>> t2 = "a"*4097
>>> s2 = "a"*4097
>>> s2 is t2
False
```

> NOTE: do the above in a REPL, if you do it in a python module, the compiler can optimise the bytecode to make them point to the same thing (not interning)
---
## References
- 