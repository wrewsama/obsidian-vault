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
 - Other primitives (variants, tuples, lists) work the same as the ocaml standard library (note: many of the library functions have different signatures e.g. `List.map`)

## Variables and Functions
- no differences from ocaml standard library
- interesting gotchas
    - when you pass a function with labelled arguments as an argument into another function, the order with which you pass in the labelled arguments matters
    - during type inference, OCaml will prefer labels over options
    - when doing partial application on a function with optional and positional arguments, if you pass in one of the positional arguments, OCaml will _erase_ all the optional arguments defined before the first positional argument
        - by _erase_, the argument isn't gone, it just becomes a normal positional argument
        - the exception is when everything is passed in at the same time (so you can pass in the optional argument at the end without it getting erased)

---
Source: https://www.goodreads.com/book/show/16087552-real-world-ocaml?ac=1&from_search=true&qid=AywbZGaVor&rank=1
