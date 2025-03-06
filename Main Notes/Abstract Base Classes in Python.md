Tags:
- [[Python]]
---
Like Abstract classes in Java
```python
from abc import ABC, abstractmethod

class Foo(ABC):
	@abstractmethod
	def bar(self, baz):
		pass
```

Will raise `TypeError` if:
- you try to instantiate Foo
- you try to instantiate an object that inherits from Foo, but doesn't implement the `bar` method

---
## References
- 