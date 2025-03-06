Tags:
- [[Python]]
---
- to have a custom hash function, need to override `__eq__` and `__hash__`, this will be hashable according to the implementation of `__hash__`
- if only `__eq__` is implemented, will be unhashable
- by default, both `__eq__` and `__hash__` use the address of the object, this is also hashable

This ensures that 'equal' elements hash to the same slot and get deduped.

Similarly, mutable objects like `list` are unhashable as `[1,2] == [1,2]` (i.e. equality is determined by value), but if you add 2 different lists to a set, they will hash to different slots, but then if you modify one to 'equal' the other, we will have duplicates in the set, which is not allowed
-  while python allows you to make a custom class that hashes based on a mutable field, (e.g. calling `hash(str(field)))` ), this same issue could occur
- however, it's okay to use the default implementations of eq and hash as the address for an object is immutable

---
## References
- 