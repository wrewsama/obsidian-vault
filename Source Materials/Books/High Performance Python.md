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

---
Source: https://www.goodreads.com/book/show/49828191-high-performance-python
