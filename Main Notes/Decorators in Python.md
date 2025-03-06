Tags:
- [[Python]]
---
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

---
## References
- 