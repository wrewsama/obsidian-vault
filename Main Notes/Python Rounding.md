Tags:
- [[Python]]
- [[Math]]
---
- built-in function to round to X d.p.
```python
# round 3.14159 to 2 d.p.
print(round(3.14159, 2)) # 3.14
```

## gotcha - Banker's Rounding
- round half to even
```python
print(round(1.5)) # 2
print(round(2.5)) # 2
print(round(-1.5)) # -2
print(round(-2.5)) # -2

# note: sometimes floating-point math messes with the rounding
# as the d.p. gets > 1
print(round(1.25, 1)) # 1.2
print(round(1.35, 1)) # 1.4
```
---
## References
- https://www.w3schools.com/python/ref_func_round.asp
- https://medium.com/@akhilnathe/understanding-pythons-round-function-from-basics-to-bankers-b64e7dd73477