Tags:
- [[Python]]
---
- for loops, the `else` block essentially executes if there is no `break`
- for `try`, the `else` block executes if there is no exception
```python
for x in arr:
    continue
else:
    print("for else") # runs if for loop runs to the end
    
i = 0
while i < 5:
    i += 1
else:
    print("while else") # runs if while condition is now false
    
try:
    pass
except Exception:
    pass
else:
    print("try else") # runs if no exception was caught
```
---
## References
- 