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

## Coupling
- no dependency cycles
- depend in the direction of stability
    - unstable (likely to change) components should depend on stable ones, not the other way around
- a component should be as abstract as it is stable
    - measured by the ratio of abstract classes/interfaces to total classes

## Architecture
- architecture of a system: shape given to the system
    - the division of the system into components
    - the arrangement of those components
    - the way those components interact
- **goal: leave as many options open as possible**
- solution: separate details (implementation) from policy (business logic)
    - i.e. make the business rules agnostic to the specific technologies (e.g. storage system)

## independence
- decoupling
    - horizontal layers: e.g. UI, business rules, database, etc.
    - vertically: i.e. different _use cases_
- modes of decoupling
    - by modules in source code
    - by deployable units (e.g. packages, jar files, etc.)
    - by services

## Boundaries
- separate into components: some in the core business and others are 'plugins' (e.g. databases, UIs)
- different components change
    - at different rates
    - for different reasons
- draw lines between the core business and the plugin components using dependency inversion => lower level details (plugins) depend on high level logic (core), not the other way around

## Business Rules
- Critical Business Rules: rules that let the business make money, would exist even if there was no software to automate them (e.g. the logic to calculate a loan)
- Entities: encapsulate small set of related critical business rules and critical business data
- Use Cases: The way the software system is used. Takes in some input, performs operations, and returns some output. Input and output should both be simple data structures without low level dependencies

## Screaming Architecture
- the architecture should "scream" out the use cases, not the framework
- software architectures are structures that support the use cases of the system
- all use cases should be unit-testable without any of the frameworks in place

## Clean Architecture
- **CORE RULE: source code dependencies must point only inward, toward higher-level policies**
- layers
    - entities
    - use cases
    - interface adapters
    - frameworks and drivers
- data is passed across boundaries in the the _inner_ circle's format

## Humble Object Pattern
- extract out hard-to-test functionality into its own, minimal module
- put the main, testable behaviours in another module - this is the _humble object_

## The Main Component
- "plugin" that handles creating and injecting dependencies, then handing over control

## Test Boundary
- tests are also part of your system
- can be thought of as the outermost ring (no dependents, depends on the whole system)
- avoid coupling by providing a testing API to hide the implementation structure of the system from the tests

## Clean Embedded Architecture
- firmware and hardware regularly become obsolete and require changes
- treat hardware and OS as a detail
    - OS abstraction layer between the app and the OS
    - hardware abstraction layer between the OS and the firmware / hardware

---
Source: https://www.goodreads.com/book/show/18043011-clean-architecture
