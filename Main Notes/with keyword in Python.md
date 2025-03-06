Tags:
- [[Python]]
---
Used for managing (creating, writing, reading, and closing) resources (files, network connections)
```python
with open('xdd.txt', 'w') as f:
	f.write('xdd')
```

ensures that the resource is closed when the block exits for any reason (including exceptions & interrupts)


---
## References
- 