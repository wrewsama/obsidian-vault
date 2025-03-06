Tags:
- [[Tags/Java|Java]]
---
1. Java code is compiled to bytecode by javac compiler
2. During runtime, class files are loaded and verified
3. Bytecode is interpreted at first while JIT converts bytecode to machine code
	1. JVM can optimise the JIT compilation due to the extra info at runtime (e.g. skipping unnecessary null checks to minimise Assembly jump instructions)
4. Once JIT is done, JVM switches to the compiled code

---
## References
- 