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
    - in practice: segregate into immutable and mutable components, keep as much logic in the immutable parts as possible

## Design Principles
Goals:
- ease of change
- understandability
- usable by many other systems

- Single Responsibility Principle
    - a module should be responsible to exactly one actor (user)
- Open-Closed Principle
    - module should be open for extension but closed for modification
    - => to extend functionality, only need to write extra code, no need to change existing
- Liskov Substitution Principle
    - an object of a subtype can be substituted for an object of a supertype with no change in behaviour
- Interface Segregation Principle
    - Avoid importing unnecessary functionality from a dependency by splitting that dependency into separate interfaces
- Dependency Inversion Principle
    - Depend on an interface / abstract class, never a concrete class

## Cohesion
- Reuse/Release Equivalence Principle
    - the granule of reuse is the granule of release
    - => modules grouped in a component should have an overarching theme that allows them to be released and reused together
- Common Closure Principle
    - group classes that change for the same reason, at the same time. The inverse applies too
- Common Reuse Principle
    - Don't force users of a component to depend on things they don't need
---
Source: https://www.goodreads.com/book/show/18043011-clean-architecture
