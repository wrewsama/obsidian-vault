## == vs is

== checks for equal VALUE

is checks if 2 things are at the same memory location

## Decorators

pattern that allows user to add new functionality to something without modifying its structure

implemented as a function that takes in a function and returns a wrapper on that function with extended functionality

## Dict/List comprehension

Syntactic sugar that lets us build mapped / filtered lists or dicts from a given iterable

## Pickling/ unpickling

Basically same idea as zipping and unzipping (compression + storage)

## Generators

Function that returns an iterable collection of items one at a time, without loading the whole collection into memory

## Execution

Actually pretty similar to java

1. compilation to bytecode (.pyc files in __pycache__)
2. at the same time...

1. interpreter
2. JIT compiler

## GIL

Global Interpreter Lock: mutex that allows only 1 thread to control Python interpreter

## with
Used for managing (creating, writing, reading, and closing) resources (files, network connections)
```python
with open('xdd.txt', 'w') as f:
	f.write('xdd')
```

ensures that the resource is closed when the block exits for any reason (including exceptions & interrupts)

## try/except
4 parts
- try: code to exec
- except: execs if the code in the try block throws the specified exception (catches all if none specified), can have multiple
- (opt) else: execs if the code in the try block doesn't throw something caught in the except block(s)
- (opt) finally: execs after everything, regardless of whether an exception was thrown

