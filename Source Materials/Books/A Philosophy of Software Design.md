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

## Define Errors Out of Existence
- exceptions thrown by a class are part of its interface
- decreasing the number of places exceptions need to be handled decreases complexity
- techniques
    - define errors out of existence: change the definition of the module such that exceptional cases don't exist (e.g. instead of "deleting a file", make it "ensure file doesn't exist" => if the file already doesn't exist, no exception)
    - exception masking: handle the exceptional cases in the implementation instead of throwing them to the caller (e.g. TCP retrying automatically)
    - exception aggregation: handle many exceptions together (e.g. grouping many calls with the same exception in the same try/catch)
    - just crash
- reversal: if the caller really needs exception information, then exceptions must still be raised

## Design It Twice
- design each module twice, with approaches that are as different as possible
- then weigh the pros and cons
- the most important factor: how easily higher-level modules can use this module's interface

## Comments
- **If users must read the code of a method in order to use it, then there is no abstraction**. Comments help add info on top of the method signature
- comments should capture info that can't be expressed in code
    - lower-level comments: enhances precision
        - e.g. add additional details to variable definitions that can't be made obvious from the name
    - higher-level comments: enhance intuition
        - e.g. giving the big-picture view of what a block of code is trying to do
- interface comments: provide info a user needs to use a class/method
- implementation comments: help user understand what the code is doing at a high-level, and why it's being done
- cross-module design comments: describe dependencies between modules that aren't obvious from the code
- write the comments before writing code

## Naming
- requirements
    - precision: should create a clear image of the underlying object
    - consistency: 
        - use the same common name for a given purpose (e.g. don't use `line_count` and `num_lines` for the same thing)
        - ensure all variables with the same name behave the same way (e.g. don't use `date` for both start and end date variables)
---
Source: https://www.goodreads.com/book/show/39996759-a-philosophy-of-software-design
