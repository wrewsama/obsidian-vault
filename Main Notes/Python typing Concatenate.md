Tags:
- [[Python]]
---
- essentially lets us make a [[Python ParamSpec]] ignore certain params

**example**
```python
def with_req(f: Callable[Concatenate[Req, P], R]) -> Callable[P, R]:
    '''takes in a func(req, others...) and returns a func(others...)'''
    def inner(*args: P.args, **kwargs: P.kwargs) -> R:
        return f(Req(), *args, **kwargs)
return inner
```

---
## References
- https://peps.python.org/pep-0612/