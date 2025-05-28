Tags:
- [[Python]]
---
## What it is
- function that takes in a function with args and kwargs and returns a version of that function with those args / kwargs fixed

## One Example Use Case
```python
from concurrent.futures import ThreadPoolExecutor
from functools import partial

def greet_user(username, greeting='Hello'):
    print(f"{greeting}! {username}")

names = ["Miko", "Suisei", "Bijou"]
hi_func = partial(greet_user, greeting='hi')

with ThreadPoolExecutor as tpe:
    tpe.map(hi_func, names)
```
Since our list only has names and we want to fix the greeting, we can use `partial` to avoid creating a new list with both the name and new greeting to get mapped with `greet_user`

---
## References
- https://docs.python.org/3/library/functools.html