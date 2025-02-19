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

## Chapter 3: Dictionaries and Sets
#### Dealing with missing keys
- `setdefault`: If `key not in hm`, store `default` in `hm[key]`. Returns the value of `hm[key]`.
- `defaultdict`: calls a provided factory function to return a default value
- `__missing__` method: for custom dict-like objects, automatically invoked when `__getitem__` raises a `KeyError`
```python
hm.setdefault(key, default=[]).append(x)

from collections import defaultdict
dd = defaultdict(int) # calls int() for missing keys
```

#### Custom mapping class
- Subclass `UserDict`
- Override special methods as needed
- Note that `UserDict` doesn't inherit from `dict`, it stores a `dict` under the `data` field

#### Immutable Mapping
```python
from types import MappingProxyType
d = {}
readonly = MappingProxyType(d)
```

#### Dictionary Views
- `d.keys()`: 'set' of all keys
- `d.values()`: 'set' of all values
- `d.items()`: 'set' of (key, value) tuples
- not actual `set` objects, but have similar functionality to immutable sets (`frozenset`)

#### Set Operations
- union: `a & b`
- intersection: `a | b`
- difference: `a - b`
- symmetric difference: `a ^ b`

#### Hashing Implementation
**algo**
1. compute hashcode
2. compute index (hashcode % table size)
3. if bucket at index is empty, store hashcode and data there
4. comparing with the hashcode and data in the bucket, if both hashcodes and both data are the same, we know the element is in the set already, skip
5. compute next index and goto 3

**set storage**
single table storing (hash code, pointer to element)

**compact dict storage**
- issue: single table allocates space for hashcode and pointers even if bucket is empty
- add layer of indirection. Store an index in the hash table and store the hashcode and pointers in a different array

**split table**
- used only to fill the `__dict__` special attribute of each instance of a class
- intuition: since the attribute names for every instance of a particular class is going to be the same (along with their hashcodes) , store 1 hashtable mapping hashcodes/keys to buckets and let each instance have its own array of values

## Chapter 4: Text vs Bytes
- `s.encode(encoding)` / `s.decode(encoding)`: convert from string to bytes and back with the provided encoding
- `bytes`: immutable binary sequence
- `bytearray`: mutable binary sequence
- `encode` has an optional parameter `errors` defining what to do upon a `UnicodeEncodeError` (i.e. an unsupported character)
	- e.g. ignore, replace, etc.
- Unicode sandwich: best practice for IO, decode bytes -> string as early as possible, encode string -> bytes as late as possible, do all processing on text
	- specify encoding like: `open(filename, 'r', encoding='utf-8')`
	- mac and Linux use utf-8 by default but Windows uses something else
- Unicode normalisation
	- issue: characters can be composed in multiple ways (e.g. `é` and `e\u0301` both render to the same thing, (U+301 is known as a combining character))
	- `unicodedata.normalize(mode, string)`
	- common modes: `NFC` composes to shortest equivalent string, `NFD` does the opposite decomposition
	- combining characters can be found with `unicodedata.combining(c)`
- sorting Unicode text:
```python
from pyuca import Collator
col = Collator()
arr.sort(key=col.sort_key) # properly treats modified letters like é as e
```

## Chapter 5: Record-like Data Structures
ways:
- `collections.namedtuple`
	- provide the names of each field in the constructor
- inheriting from `typing.NamedTuple`
	- declare the variable names along with type hints like a dataclass
- annotating class with `@dataclasses.dataclass`

dataclass example:
```python
from dataclasses import dataclass, field

# can set options in annotation (e.g. immutability, whether to generate eq/repr/etc. methods)
@dataclass(frozen=True) 
class DC:
	a: int
	b: float = 3.5 # all params with default vals must be grouped at the bottom
	c: list = field(default_factory=list) # default vals must be immutable
```

`__post_init__(self)`: runs after constructor is called
`dataclasses.InitVar`: variable that's only passed into the constructor for initialisation, should not be a field

`struct` module: convert fields of bytes into Python tuples and back
- usage: call `struct.unpack(FORMAT, data)` where `FORMAT` is a string representing the types of each field
- **should only be used for legacy support**
- alternatives for binary exchange:
	- `pickle`: only other Python apps
	- protobuf or MessagePack or JSON: multiplatform 

## Chapter 6: References, Mutability, Recycling
- `==` compares values, `is` compares identities
- deep copies (don't share references of nested objects): `copy.deepcopy` (also handles cyclic references)
- function parameters are passed by reference by default

the classic mutable parameter default mistake:
```python
# WRONG
def foo(arr=[]): # every call to foo references this same list
	pass

# CORRECT
def foo(arr=None):
	if arr is None:
		arr = []
	pass
```

- `del` deletes _names_, not _objects_
- objects are only removed when the GC sees that:
	- no more references (reference-counting)
	- object is unreachable (generational GC)
- special case: _weak references_(`weakref`) don't count as a 'reference' by the GC (i.e. if the only references left on an object are weak, the object is still GC'ed)