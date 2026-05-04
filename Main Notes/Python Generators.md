Tags:
- [[Python]]
---
## what
- Function that returns an iterable collection of items one at a time, without loading the whole collection into memory
- also enables bidirectional communication using `.send()` [[Python Generator send]]

## type hinting
- `-> Generator[YieldType, SendType, ReturnType]`
- usually if you're only yielding values, just `-> Iterator[YieldType]`
---
## References
- https://www.datacamp.com/tutorial/python-generators
- [typing docs](https://docs.python.org/3/library/typing.html#annotating-generators-and-coroutines)