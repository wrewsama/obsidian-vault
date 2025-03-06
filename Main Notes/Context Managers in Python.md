Tags:
- [[Python]]
---
Defines behaviour when used with the [[with keyword in Python]]
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

---
## References
- 