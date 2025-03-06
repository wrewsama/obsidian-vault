Tags:
- [[Python]]
---
objects are instances of a class
classes are instances of metaclasses

- created by inheriting from the `type` class
- can override dunder methods to change behaviour of the classes that use this as a metaclass
	- control instantiation `__call__`
	- modify the namespace of the class `__prepare__`

---
## References
- 