Tags:
- [[Python]]
---
## what
- formatted string literals
- embed variables into strings e.g. `f"hi {name}!"`

## layout control
- alignment/padding
    - `f"{text:=<10}"`: pad with `=` till width=10, left align
    - `>`: right align
    - `^` center
    - if the padding character isn't specified, will use space
- numbers
    - precision
        - `f"{num:.2f}"`: round `num` to 2dp
    - thousands separator
        - `:,`: separate thousands with `,`
    - percentages
        - `:%"
    - base
        - `:b`: binary
        - `:X`: hex
- self-documentation
    - `"{some_var=}"`: will return `"some_var=<VALUE>"`
---
## References
- https://www.geeksforgeeks.org/python/string-alignment-in-python-f-string/