Tags:
- [[Python]]
---
## What are they
- basically class-based [[Getters and Setters in Python]], lower level than `@property` decorators
- one advantage is reusability across different classes
- disadvantage is the extra boilerplate
```python
class Desc:
    def __get__(self, instance, instance_type=None):
        return 35
        
    def __set__(self, instance, value):
        # set something
        pass

class Foo:
    desc = Desc()

foo = Foo()
print(foo.desc) # 35
```

---
## References
- https://www.geeksforgeeks.org/python/descriptor-in-python/