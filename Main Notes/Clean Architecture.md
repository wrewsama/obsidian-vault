Tags:
- [[Software Design]]
---
## What is Design and Architecture
- there is no division between the two
- both are just a series of decisions from high to low level
> The only way to go _fast_, is to go _well_ - Uncle Bob

## Two Values
- Behaviour: what the software needs to do is urgent but not so important
- Architecture: the way the software is designed is important but not urgent

## Paradigms
- each paradigm adds restrictions, not capabilities
- structured programming
    - restrict direct transfer of control (no `goto`)
- object oriented programming
    - restrict indirect transfer of control (replacing "polymorphism" via function pointers with real polymorphism)
    - enables dependency inversion
- functional programming
    - restrict assignment (immutability)
---
Source: https://www.goodreads.com/book/show/18043011-clean-architecture
