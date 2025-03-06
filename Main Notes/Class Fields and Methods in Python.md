Tags:
- [[Python]]
---
Like static fields / methods in Java
`@staticmethod` also exists, but it doesn't take in the class as the first argument, hence it can't access class fields

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


---
## References
- 