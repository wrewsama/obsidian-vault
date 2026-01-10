Tags:
- [[Software Engineering]]
- [[Software Design]]
---
## The Nature of Complexity
> **Complexity**: Anything related to the structure of a software system that makes it hard to understand and modify the system
- Mathematically, $C = \sum C_{p} t_{p}$ (sum of complexity of each part * time devs spend working on that part)
- symptoms
    - a change requires code modification in many places
    - a change requires much prerequisite knowledge
    - not obvious which code to change or what information to get in order to perform a change
- causes
    - dependencies
    - obscurity
- key insight: Complexity is caused by the accumulation of many tiny issues over time, not a single catastrophic design error

## Modules Should be Deep
- Goal of modular design: minimise dependencies between modules
- Abstraction: simplified representation of an entity, omitting unimportant details
- Deep modules: small interface, large amount of functionality
    - imagine interface being the surface area and functionality being the volume of a pool of water - we want it to be _deep_
    - in other words, it _hides_ a lot of complexity
- shallow modules can do more harm than good, don't split your modules so much they become shallow 
- interfaces should be optimised to simplify the common use as much as possible

---
Source: https://www.goodreads.com/book/show/39996759-a-philosophy-of-software-design
