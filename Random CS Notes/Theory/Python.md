## == vs is

== checks for equal VALUE

is checks if 2 things are at the same memory location

## Decorators

pattern that allows user to add new functionality to something without modifying its structure

- usually implemented as a function that takes in a function (or class) and returns a wrapper on that function (or class) with extended functionality
- (also can be implemented as a class that takes in the thing to be extended in `__init__` and overrides the `__call__` method)

**examples**
```python
def log_methods(cls):  
	for name, value in vars(cls).items():  
		if callable(value):  
			setattr(cls, name, log_method(value))  
	return cls  
​  
def log_method(func):  
	def wrapper(*args, **kwargs):  
		print(f"Calling method: {func.__name__}")  
		return func(*args, **kwargs)  
	return wrapper  
​  
@log_methods  
class MyClass:  
	def my_method(self):  
		print("Hello from my_method!")
```

**decorator with args**
take in the args and return a decorator
```python
def deco_with_args(arg1):
	def deco(func):
		def wrapper(*args, **kwargs):
			print(f'decorated with {arg1}')
			return func(*args, **kwargs)
		return wrapper
	return deco

@deco_with_args(35)
def foo():
	pass
```

**decorator with optional args**
works with both `@dec` and `@dec(arg=123)`
```python
def deco_with_args(func=None, arg1=69):
    def deco(func):
        def wrapper(*args, **kwargs):
            print(f'decorated with {arg1}')
            return func(*args, **kwargs)
        return wrapper

    if func is not None:
        return deco(func)
    return deco

@deco_with_args(arg1=35)
def foo():
	pass
@deco_with_args
def bar():
	pass
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
2. Interpreter/PVM executes the bytecode line-by-line

## PVM
Python Virtual Machine
handles:
- Memory management (including GC)
- Executing instructions

## GIL
Global Interpreter Lock: mutex in the CPython interpreter that allows only 1 thread to execute bytecode

**Purpose**
- Needed because GC uses reference counting
- Prevents race conditions if 2 threads access the same object
- Fine-grained locking for every single object incurs large overhead and could potentially lead to deadlocks

**Implications**
- multithreading doesn't help CPU-bound tasks (though IO-bound tasks can still benefit since they aren't executing bytecode while waiting, so other threads can run instead)

**Solution: Multiprocessing**
- creates multiple OS processes
- each process has its own instance of the interpreter
- can now execute CPU bound tasks at the same time (on different cores)
- higher overhead
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
another note: thanks to the [[#GIL]], all GC is stop-the-world

## Class fields / methods
Like static fields / methods in [[Java]]
`@staticmethod` also exists, but it doesn't take in the class as the first arg, hence it can't access class fields

example:
```python
class Counter:
	count = 0
	@classmethod
	def increment(cls):
		cls.count += 1
		
Counter.increment()
print(Counter.count) # Output: 1
```

## Abstract Base Classes
Like Abstract classes in Java
```python
from abc import ABC, abstractmethod

class Foo(ABC):
	@abstractmethod
	def bar(self, baz):
		pass
```

Will raise `TypeError` if:
- you try to instantiate Foo
- you try to instantiate an object that inherits from Foo, but doesn't implement the `bar` method

## Metaclasses
objects are instances of a class
classes are instances of metaclasses

- created by inheriting from the `type` class
- can override dunder methods to change behaviour of the classes that use this as a metaclass
	- control instantiation `__call__`
	- modify the namespace of the class `__prepare__`


## asyncio
Letting I/O tasks run in the background without blocking to wait for them to complete

- use `async def` to define a coroutine
	- this can also be used to define an async generator, which you can loop through with `async for`
	- you can `async def` `__aenter__` and `__aexit__` to make a async context manager, which can be entered with `async with`
- when running the program, use `asyncio.run` to start the event loop
- schedule the tasks using `asyncio.gather` or `asyncio.create_task`
	- example: `asyncio.gather(f1(), f2())`
- the event loop
	- pick a ready task
	- if the task encounters an`await`, it yields control back to the event loop
- event loop runs on a single thread	

## threading / multiprocessing
- create a thread: `t = threading.Thread(target=<FUNCTION>, args=<TUPLE OF ARGS>, kwargs=<DICT OF KWARGS>)`
- start running a thread: `t.start()`
- wait for a thread to complete: `t.join()`


|                     | threading                                                                                | multiprocessing                                 |
| ------------------- | ---------------------------------------------------------------------------------------- | ----------------------------------------------- |
| create              | `t = threading.Thread(target=<FUNCTION>, args=<TUPLE OF ARGS>, kwargs=<DICT OF KWARGS>)` | `p = multiprocessing.Process`, args are similar |
| start               | `t.start()`                                                                              | `p.start()`                                     |
| wait for completion | `t.join()`                                                                               | `p.join()`                                      |

## Hashable objects

- to have a custom hash function, need to override `__eq__` and `__hash__`, this will be hashable according to the implementation of `__hash__`
- if only `__eq__` is implemented, will be unhashable
- by default, both `__eq__` and `__hash__` use the address of the object, this is also hashable

This ensures that 'equal' elements hash to the same slot and get deduped.

Similarly, mutable objects like `list` are unhashable as `[1,2] == [1,2]` (i.e. equality is determined by value), but if you add 2 different lists to a set, they will hash to different slots, but then if you modify one to 'equal' the other, we will have duplicates in the set, which is not allowed
-  while python allows you to make a custom class that hashes based on a mutable field, (e.g. calling `hash(str(field)))` ), this same issue could occur
- however, it's okay to use the default implementations of eq and hash as the address for an object is immutable

## Method Resolution Order 
- for multiple inheritance
- searching for a method begins with the class itself, and then for parent classes from left to right
- kind of like a dfs

## WSGI
Web Server Gateway Interface: defines standard interface between web servers (e.g. Nginx) and Python web apps

**Example: Gunicorn**
_pre-fork_ worker model
- creates a master process and multiple child (worker) processes on startup
- master process creates the welcome socket for incoming connections
- worker processes
	- accept connections on the welcome socket
	- handle requests
- can also use other concurrency methods like threads and async

**Why WSGI is needed for Python**
- circumvent the GIL
- no built-in http serving capabilities (e.g. Go's `net/http` package)

## Getters and Setters
Goal: encapsulation, control the access of a field in the object

possible use cases:
- make field read-only
- perform data validation (setter)

example:
```python
class Example:
	def __init__(self):
		self._value = 0

	@property
	def value(self):
		return self._value

	@value.setter
	def value(self, new_value):
		if new_value < 0:
			raise ValueError("Value must be non-negative")
		self._value = new_value
```
