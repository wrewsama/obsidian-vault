Tags:
- [[Python]]
---
- Type information is bound to the values rather than the variables, (unlike in static-typed languages where types are bound to the variables in a symbol table)
- type checking is done at runtime (unlike in static-typed languages where the checking can be done at compile time)
- Every value is represented as an object, and each object contains
	- reference count (for gc)
	- pointer to its corresponding type object
		- type object contains name, memory layout, and associated methods/attributes
	- actual data of the object

---
## References
- 