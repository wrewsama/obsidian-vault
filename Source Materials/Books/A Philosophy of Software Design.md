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

## Information Leakage
- info leakage occurs when: 2 classes need a certain piece of information (e.g. a file format)
- that information should be hidden in a single class, no other class needs to have that
- often caused by temporal decomposition
    - naively splitting a sequence of operations into classes for each step
    - e.g. `FileReader, FileModifier, FileWriter`
    - here the reader and writer need information like file path, file format, etc.
    - hence, read and write steps should be 1 class
    - When designing modules, **focus on info needed for the task, not the order of the tasks**

## General Purpose Modules are Deeper
- _functionality_ should serve current requirements (in line with YAGNI)
- however, the _interface_ should be as general enough to support multiple uses
- special case handling is a red flag - should minimise code like `if condition do otherThing` 

## Different Layer, Different Abstraction
- every piece of code (interface, class, function, etc.) adds complexity
- hence it must hide more complexity than it adds
- the interface should be at a higher level of abstraction than its implementation
- this lets it hide complexity from upper layers, which can utilise it like a black box

## Pull Complexity Downwards
- If you can only pick one, **Simple Interface > Simple Implementation**
    - handle complexity in the implementation instead of leaving it in the interface for users to handle
- Caveat: only pull downwards if it reduces overall complexity (non-example: pulling unrelated complexity into your implementation can make everything more complex)

## Together or Apart
- bring separate modules together if:
    - they rely on shared information
    - the combined interface is simpler to use than the 2 separate interfaces
    - they duplicate similar logic
- split a module into different modules if:
    - it contains both general purpose code and the specialised uses of that code
    - the module has more than 1 responsibility
---
Source: https://www.goodreads.com/book/show/39996759-a-philosophy-of-software-design
