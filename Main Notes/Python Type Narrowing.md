Tags:
- [[Python]]
---
## why
- if you need to narrow a type based on it's runtime value, you can use `isinstance`
- however, if you return the result of that `isinstance` in a custom function, type checkers can't "remember" what the narrowed type is
## what
- `TypeGuard[T]`: if true, X is the narrowed type, else, it's the wide type
- `TypeIs[T]`: if true, X is the narrowed type, else, it's the wide type _minus that narrowed type_
- `TypeIs` is obviously more powerful than `TypeGuard`, but it's only available from 3.14 onward while `TypeGuard` is available from 3.10 onward
## example
> note: typecheckers (e.g. in LSP) will show the types, running the snippet won't show all of them

 ```python
import typing

def foo() -> int | str:
    return "" 

def naive_is_int(x: object) -> bool:
    return isinstance(x, int)

def typeguard_is_int(x: object) -> typing.TypeGuard[int]:
    return isinstance(x, int)

def typeis_is_int(x: object) -> typing.TypeIs[int]:
    return isinstance(x, int)

what_am_i = foo()

if naive_is_int(what_am_i):
    typing.reveal_type(what_am_i) # int | str

if typeguard_is_int(what_am_i):
    typing.reveal_type(what_am_i) # int

if not typeguard_is_int(what_am_i):
    typing.reveal_type(what_am_i) # int | str

if typeis_is_int(what_am_i):
    typing.reveal_type(what_am_i) # int

if not typeis_is_int(what_am_i):
    typing.reveal_type(what_am_i) # str

 ```
---
## References
- 