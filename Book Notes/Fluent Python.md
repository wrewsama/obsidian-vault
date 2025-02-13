## Chapter 1: The Python Data Model
Python Data Model: API used to make custom objects work with existing idiomatic language features (e.g. `len()`, `for x in arr`, etc.)

common special methods:
- `__abs__(self)`: absolute value returned when calling `abs(obj)`
- `__add__(self, other)`: +
- `__mul__(self, other)`: *
- `__repr__(self)`: string representation (note that `print` calls `str(obj)` which will call `__repr__` if `__str__` is not defined.)
	- should be unambiguous (e.g. print strings as `'foo'` instead of just `foo`)
	- use fstrings like this `f'{var!r}'` to get the `__repr__` representation
- `__bool__(self)`: boolean contexts e.g. `if obj: ...`, `while obj: ...`
- `__getitem__(self, position)`: `obj[pos]`, also slicing, iterating, sorting, etc.
- `__iter__(self)`: return an iterator through the object, used for for loops, comprehensions, and unpacking with the `*`
- `__len__(self)`: `len(obj)`
- `__contains__(self, item)`: return boolean whether `item in obj`
