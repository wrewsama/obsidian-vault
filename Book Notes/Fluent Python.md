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

## Chapter 2: Sequences
#### List Comprehensions

```python
arr = [-1, 2, 3]
mapped = [abs(x) for x in arr]
filtered = [x for x in arr if x > 0]
```

#### Tuples
uses:
- immutable lists: references in the tuple cannot be changed (but the objects they point to can)
- records: stores data, position of the item in the record gives it its meaning

other differences from list:
- allocated exact memory needed while list has some extras (for future appends)
- references to items are stored inside the tuple struct itself while list stores a pointer to an array with the references (for dynamic resizing)
- when evaluating a tuple, the generated Python bytecode loads the whole tuple in 1 operation while lists need to loaded element by element, then build the list

unpacking examples:
```python
a, *b, c, d = range(5) # using * to get extra items
# 0 [1, 2] 3 4
a, (b, c), d = (1, (2, 3), 4) # unpacking nested tuples
# 1 2 3 4
```

#### Slicing
- `arr[start:stop:step]` is equivalent to `arr[slice(start, stop, step)]`. This slice object can be kept in a variable and reused
- slicing can be used to reassign and delete parts of the sequence

#### + and * on sequences
- `['x']*3` evaluates to `['x', 'x', 'x']`
- uses `__imul__` by default, falls back to `__mul__`
- be careful of using a mutable object in place of 'x', all the entries will point to that same object
- `+` just returns a new list that is the concatenation of 2 lists
- uses `__iadd__` by default, falls back to `__add__`

#### bisect
- `bisect.bisect(haystack, needle)` binary searches a sorted sequence `haystack` for a place to insert `needle` (this will be at the far right of all the `needle` duplicates)
- `bisect.bisect_left` does the same, but on the far left of all the `needle` duplicates
- `bisect.insort(seq, item)` inserts `item` at the appropriate location in a sorted `seq`, O(n)

#### Arrays
- static type, provided by a string typecode in the constructor 
- stores the actual variable data, not just references => more efficient memory usage
- same methods as mutable sequences, plus `tofile` and `fromfile`

#### Memory Views
- shared-memory sequence type
- create a `memoryview(arr)`
- `cast` it to different shapes
- the views can be manipulated and the underlying view will change