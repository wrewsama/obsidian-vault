Tags:
- [[Python]]
---
## what
- class to group multiple dictionaries in layers
- pass in a variable number of dicts into the constructor
    - can add more afterwards with the `new_child` method
- keys will be looked up in the order which the dicts were passed in, returning the first value found

## example
```python
from collections import ChainMap

dict1 = {"a": 1}
dict2 = {"a": 2, "b": 5}
chain = ChainMap(dict1, dict2)
print(chain["a"]) # 1
print(chain["b"]) # 5
```

## tricky part: updates
- all updates are applied _only to the first dict_, regardless of which dict has the key
```python
# continuing above example
chain["a"] = 10
chain["b"] = 20
print(dict1) # {"a": 10, "b": 20}
print(dict2) # {"a": 2, "b": 5} (unchanged)
```
---
## References
- https://realpython.com/python-chainmap/
- https://getcracked.io/question/1389/put-on-chains?language=Python