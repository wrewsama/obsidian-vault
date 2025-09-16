Tags:
- [[Python]]
---
- "mix ins"
- class that provides methods and data to other classes through inheritance
- for use in multiple inheritance
- probably goes against [favouring composition over inheritance](https://en.wikipedia.org/wiki/Composition_over_inheritance)

```python
class FooMixin:
    def foo(self):
        doSomething()

class BarMixin:
    def bar(self):
        doSomethingElse()
        
class Baz(FooMixin, BarMixin):
    pass

baz = Baz()
baz.foo()
baz.bar()
```
---
## References
- https://www.linkedin.com/pulse/understanding-mixins-python-powerful-tool-code-joseph-kalapurackal/