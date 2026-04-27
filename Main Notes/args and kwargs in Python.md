Tags:
- [[Python]]
---
## what
- args: tuple of all "extra" positional arguments
- kwargs: dictionary of all "extra" keyword arguments, mapping keyword to value
- "extra": passed in but not explicitly named in the function definition

## rules
- at most one `args` and one `kwargs`
- positional args before `*args`
- keyword args before `**kwargs`
- `*args` before `**kwargs`

## example
```python
def foo(a, b=1, *args, c=10, **kwargs):
	print(f'args = {args}') # (123,)
	print(f'kwargs = {kwargs}') # {'d': 'hello'}
foo('hi', 35, 123, c=42, d='hello')
```

---
## References
- 