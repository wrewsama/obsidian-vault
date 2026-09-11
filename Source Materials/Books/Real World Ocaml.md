Tags:
- [[Ocaml]]
---
## Guided Tour
 - `Base` vs `Core`
     - Base: lightweight, portable standard library
     - Core: extension of Base with extra useful modules (e.g. data structures, time, etc.), only usable on UNIX-like systems
 - In Base/Core, operators (e.g. `=`, `+`, etc.) are scoped to each module
     - e.g. `=` only works on integers, need to use `String.(str1 = str2)` for string comparison
     - can do `let open Float.O in float1 + float2 = float3`
     - `+.` also works without opening `Float.O`
     - `**` is integer exponentiation (was float exp in the std lib)
     - `**.` is float exponentiation (also available in `Float.O`)
 - Other primitives (variables, functions, variants, tuples, lists) work the same as the ocaml standard library

---
Source: https://www.goodreads.com/book/show/16087552-real-world-ocaml?ac=1&from_search=true&qid=AywbZGaVor&rank=1
