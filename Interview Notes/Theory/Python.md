## == vs is

== checks for equal VALUE

is checks if 2 things are at the same memory location

## Decorators

pattern that allows user to add new functionality to something without modifying its structure

implemented as a function that takes in a function and returns a wrapper on that function with extended functionality

```python
def decorator_function(original_function):
	def wrapper_function(*args, **kwargs):
		# Do something before the original function is called
		result = original_function(*args, **kwargs) 
		# Do something after the original function is called
		return result

	return wrapper_function
```

## args and kwargs
args: tuple of all arguments
kwargs: dictionary mapping keyword to value

example:
```python
def foo(x, y, test=None):
	print(f'args = {args}') # ('hi', 35)
	print(f'kwargs = {kwargs}') {'test': 'xdd'}
foo('hi', 35, test='xdd')
```

## Dict/List comprehension

Syntactic sugar that lets us build mapped / filtered lists or dicts from a given iterable

## Pickling/ unpickling

Basically same idea as zipping and unzipping (compression + storage)

## Generators

Function that returns an iterable collection of items one at a time, without loading the whole collection into memory

## Execution

Actually pretty similar to java

1. compilation to bytecode (.pyc files in __pycache__)
	- Generating Abstract Syntax Tree (AST)
	- generating .`pyc` bytecode (this is done on-demand, as needed by the PVM)
2. Interpreter in the PVM executes the bytecode line-by-line

## PVM
Python Virtual Machine
handles:
- Memory management (including GC)
- Executing instructions

## GIL
Global Interpreter Lock: mutex that allows only 1 thread to control Python interpreter

## with
Used for managing (creating, writing, reading, and closing) resources (files, network connections)
```python
with open('xdd.txt', 'w') as f:
	f.write('xdd')
```

ensures that the resource is closed when the block exits for any reason (including exceptions & interrupts)

## Context Managers
```python
class ContextManager():
    def __init__(self):
		# init method
         
    def __enter__(self):
		# called when we enter the with block, returns the manager object
		return ...
     
    def __exit__(self, exc_type, exc_value, exc_traceback):
		# called when the block is exited
		# cool note: if there's an exception but this method returns True, no exception will be raised when exiting the block
 
with ContextManager() as manager:
    print('with statement block')
```
## try/except
4 parts
- try: code to exec
- except: execs if the code in the try block throws the specified exception (catches all if none specified), can have multiple
- (opt) else: execs if the code in the try block doesn't throw something caught in the except block(s)
- (opt) finally: execs after everything, regardless of whether an exception was thrown

## How dynamic types are handled

- Type information is bound to the values rather than the variables, (unlike in static-typed languages where types are bound to the variables in a symbol table)
- type checking is done at runtime (unlike in static-typed languages where the checking can be done at compile time)
- Every value is represented as an object, and each object contains
	- reference count (for gc)
	- pointer to its corresponding type object
		- type object contains name, memory layout, and associated methods/attributes
	- actual data of the object

## Garbage collection

Python uses a combination of 2 strategies: reference counting and generational mark & sweep

**reference counting**
* Every value is represented as an object with an associated reference count
- Once it reaches 0, it is immediately freed

**Generational mark and sweep**
- helps to find and free cyclic references
- objects are divided into 3 generations
	- young, middle, old
- once a generation reaches a certain threshold, a GC process is triggered on that generation
- traces all objects starting from the root references (whatever's in stack or data memory) and marks them as reachable, free anything not marked as reachable
- any survivors in that generation get promoted to an older generation (unless they're already in the old gen)

random note: `gc.collect()` runs GC on all gens