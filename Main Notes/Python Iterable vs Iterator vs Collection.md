Tags:
- [[Python]]
---
## definitions
- **iterable**: object with the `__iter__` method that returns an iterator
- **iterator**: object with the `__next__` method that returns the _next_ item when called and raises `StopIteration` when there are no more items
    - iterators also have an `__iter__` method that returns itself, so _iterators are also iterables_
- **collection**: object with `__iter__`, `__contains__` (check if something is inside with `in`), and `__len__` (check size)
    - the presence of `__len__` also implies that we can iterate through it multiple times

## examples
- collections (also iterables)
    - dict, set, list
- iterators (also iterables)
    - generators

---
## References
- https://stackoverflow.com/questions/9884132/what-are-iterator-iterable-and-iteration
- https://docs.python.org/3/tutorial/classes.html#iterators
- https://docs.python.org/3/library/collections.abc.html