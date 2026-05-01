Tags:
- [[Python]]
---
- builtin function that lets you access methods and fields from a superclass (more specifically, the next one in the MRO)
- returns temporary object of that type
- e.g. `super().__init__(args)`
- `super(type, obj_or_type)` will search the MRO of `obj_or_type`, and get the one after `type`
---
## References
- https://realpython.com/ref/builtin-functions/super/