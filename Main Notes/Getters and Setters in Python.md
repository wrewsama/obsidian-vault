Tags:
- [[Python]]
---
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

---
## References
- 