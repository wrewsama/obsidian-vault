Tags:
- [[Python]]
---
args: tuple of all arguments
kwargs: dictionary mapping keyword to value

example:
```python
def foo(x, y, test=None):
	print(f'args = {args}') # ('hi', 35)
	print(f'kwargs = {kwargs}') # {'test': 'xdd'}
foo('hi', 35, test='xdd')
```

---
## References
- 