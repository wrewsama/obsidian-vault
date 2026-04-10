Tags:
- [[Python]]
---
- Like static fields / methods in Java
- For methods, use the builtin `classmethod` decorator and add the class as the first argument.
- `@staticmethod` also exists (builtin to python), but it doesn't take in the class as the first argument

example:
```python
class Counter:
	count = 0
	@classmethod
	def increment(cls):
		cls.count += 1
        
    @staticmethod
    def increment2():
        Counter.count += 1
		
Counter.increment()
print(Counter.count) # Output: 1
Counter.increment2()
print(Counter.count) # Output: 2
```

small gotcha: class fields are a different scope from instance methods, so while you can read them through `self`, writing to them will create a new instance field.
(see [[Python Scoping]] for more)

```python
class Counter:
    count = 0
    def get_count(self):
        return self.count # reads Counter.count
    def set_count(self):
        self.count += 1 # creates new `count` field for the instance

c = Counter()
print(c.get_count()) # Output: 0
c.set_count()
print(c.count) # Output: 1
print(Counter.count) # Output: 0
```

---
## References
- 