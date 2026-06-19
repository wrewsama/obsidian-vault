Tags:
- [[Python]]
---
Actually pretty similar to [[Java Execution]]

-  compilation to bytecode (`.pyc` files in `__pycache__`)
	- Generating Abstract Syntax Tree (AST)
	- generating .`pyc` bytecode (this is done on-demand, as needed by the [[PVM]])
- Interpreter/PVM 
    - executes the bytecode line-by-line
    - dynamically optimises bytecode in-memory (e.g. `LOAD`s on the same thing)

---
## References
- 