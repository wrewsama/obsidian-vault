Tags:
- [[Python]]
---
## Understanding Performant Python
- Fundamental parts of the computer
    - Computing units
        - e.g. CPU, GPU
        - performance measured by
            - instructions per cycle (IPC)
            - cycles per second (clock speed)
        - vectorisation / SIMD: 1 instruction operates on multiple data inputs at the same time
        - Hyperthreading: adds extra virtual CPUs to interleave executions
        - but GIL forces only 1 instruction to run at a time
    - Memory Units
        - broadly speaking, anything that stores bits
        - includes RAM, registers, hard drive, L1/L2 caches, etc.
    - Communications layers
        - connects components together
        - frontside bus: connects RAM and L1/L2 cache
        - PCI bus: for peripherals like GPU to the rest of the computer, slower than frontside bus
- how to BE a performant programmer
    - process: make it work (just good enough), make it right (clean up and add tests and docs), then make it fast (optimise)

## Profiling tools
- pure python: decorators + `time.time()`
- `/usr/bin/time` shell command with `--verbose`: for whole script
- `cProfile` python module: per-function stats
    - `snakeviz` can visualise the output
- `line_profiler` module's `@profile` decorator: per-line latency stats
- `memory_profiler` module's `@profile` decorator:  per-line memory stats
- `pyspy`: introspect running python process
- `dis`: disassembler module that allows you to inspect bytecode

## Lists and Tuples
- list sorting
    - uses Tim sort: hybrid of insertion and mergesort
    - uses `__eq__` and `__lt__` methods of the underlying objects
- list comprehensions are better than for loop + append
    - less resizing => less mem usage and time taken
    - fewer python statements => faster
- tuples: fixed size => exactly enough memory allocated, no need extra buffer for amortised O(1) inserts

## Dictionaries and Sets
- hash collision handling: probing
- the actual key/value objects are stored in an array
- the indexes of the objects in the array corresponding to the key and value are then stored in the hash table

doing
```python
import foo
foo.bar()
```
incurs an additional dictionary lookup and thus extra time taken as compared to:
```python
from foo import bar
bar()
```

## Iterators and Generators
```python
for i in foo:
    bar(i)

# is equivalent to
it = iter(foo) # convert to an iterator
while True:
    try:
        i = next(it)
    except StopIteration:
        break
    else:
        bar(i)
```

## Matrix and Vector Computation
- issue with python: doesn't support vectorisation
    - lists only store pointers, actual data needs an additional lookup => can't easily get chunks of data at the same time
    - bytecode itself isn't optimised for vectorisation (because of its dynamic nature)
- `numpy`: stores data in contiguous chunks of memory, and uses C
- `numexpr`
    - numpy can only do 1 operation at a time: e.g. $(A*B+C)$ will first make a temp vector = $A*B$, then add $C$
    - `numexpr.evaluate` can take the entire expression and compile it to efficient code
- `pandas`: efficient data manipulation for tabular data

## Compiling to C
- Compiling to C gives the best results for code that is
    - CPU bound
    - lots of loops repeating the same operations
- Cython
    - ahead-of-time (AOT) Compiler
    - use a `setup.py` script to compile a `.pyx` file
    - `.pyx` files are essentially python code with C-style type annotations (e.g. `cdef unsigned int x`)

---
Source: https://www.goodreads.com/book/show/49828191-high-performance-python
