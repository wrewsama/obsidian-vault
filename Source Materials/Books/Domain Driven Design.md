Tags:
- [[Software Design]]
---
## Introduction
- Model: simplification of reality that filters out irrelevant details
- Domain: business sector your product is to be used in
- The Heart of Software: the ability to solve domain-related problems for the user

## Crunching Knowledge
- effective domain modelers "crunch knowledge" - taking in massive amounts of domain info and distilling it down to only the essential parts
- use a mindset of continuous learning: talk to domain experts, work on development, and refine the model

## Language
- the domain model forms the basis for a single, common language for different teams (domain experts, users, dev teams, etc.) to communicate
- try out the model by using it to describe scenarios in words
- informal UML diagrams are good at showing relationships and interactions between objects, but not their conceptual definitions
- diagrams should be minimal, don't overwhelm readers with too much info
- details should be captured in clear code
- **the model is not the diagram**, the diagram only helps visualise it

## Binding Model and Implementation
- Model-driven design: using a single model for
    - analysis: reflecting the concepts in the business domain
    - design: reflecting the way the implementation is structured
- design a portion of the software to literally replicate the domain model
- iterate on the model to make it more natural to implement in code
- any technical person contributing to the model must spend time touching the code

## Isolating the Domain
- keep domain related code isolated in its own layer
    - easier to find
    - easier to reason about
- layered architecture
    - presentation
    - application
    - domain
    - infrastructure
- high cohesion within each layer
- low coupling between layers

## Model Expressed in Software
**Associations**
- for every traversable association in the model, there is a mechanism in the software with the same properties
- how to simplify associations (to make implementation simpler)
    - impose a traversal direction (e.g. `Country --president--> Person` is more useful than `Person --president of--> Country`)
    - add qualifier to reduce multiplicity
        - a country can have many presidents
        - but a country has exactly one president in a given time period
    - eliminate nonessential associations

**Entities**
- An object defined by its identity (_which instance_ it is) across time and representations
- e.g. a `Customer` is defined by who the underlying customer is
- needs a way to establish its identity (i.e. some id function)

**Value Objects**
- An object defined by _what_ it is
- e.g. you care if an `Integer` is $4$, but you don't care "which" $4$ it is

**Services**
- An operation provided as an interface
- Defined by what it can do for a client
- characteristics
    - can't be fit into an entity or value object
    - interface defined in terms of domain model
    - stateless

**Modules**
- a way to group classes
- serves to communicate to other devs that these classes should be thought of together

## Domain Object Lifecycle
- challenges
    - maintain constraints throughout
    - minimise complexity from lifecycle management
    
**Aggregates**
- group of related objects treated as a single unit
- one entity is the root
- objects outside the aggregate can only access the root, not the other objects in the aggregate
- since the root controls access to all interactions, it can be responsible for maintaining consistency within the group

**Factories**
- abstracts away the creation of complex objects (especially aggregates)
- requirements
    - atomic creation of objects / aggregates
    - enforce all invariants 
    - use an abstraction other than the concrete class(es) created
- invariant checking can be done either in the factory or delegated to the object being constructed
    - if object is mutable, best to put it in the object so it maintains its invariants even after modification
    - if object is immutable, it becomes unnecessary, so putting it in the factory abstracts away the checks and keeps the objects simpler

**Repositories**
- abstracts away the data retrieval logic
- for persistent objects that need to be
    - globally accessible
    - searchable via object attributes
- guidelines
    - since the repository is decoupled, can improve the implementation easily (caching, query optimisation, etc.)
    - leave transaction control to the client

## Refactoring Toward Deeper Insight
- make modest improvements to deepen the model
- deep models: elucidate the most important issues faced by the domain experts, filtering out the rest
- focus on:
    - knowledge crunching
    - enhancing the `ubiquitous language`

## Making Implicit Concepts Explicit
- find missing concepts
    - terms used by domain experts to concisely refer to complicated logic 
    - awkward separation of concerns between the existing objects
- make constraints explicit
    - give it its own method in the entity it concerns
    - create a `Specification` for it (if it can't be fit into a single entity)

## Supple Design
- Intention-Revealing Interfaces
    - names should reflect the responsibility of the class / method
    - but not reveal the implementation
- Side-Effect-Free Functions
    - function: operation that returns value without changing any external state
    - command: operations that change system state
    - put as much logic into functions, keep commands simple
- Assertions
    - communicate preconditions and postconditions regarding the system state
    - either through `assert`s in the code or though unit tests
- Conceptual Contours
    - the way classes are broken down and organised in the code should reflect how they are in the domain
- Standalone Classes
    - make sure each class can be comprehensible without having to look at other classes
    - i.e. reduce coupling
- Closure of Operations
    - use an operation that returns the same type as its argument(s)
    - provides functionality without introducing extra dependencies

## Design Patterns as Domain Patterns
- design patterns that can enhance the domain model, rather than just the technical details, can work as domain patterns
- e.g.
    - composite
    - strategy

## Maintaining Model Integrity
- bounded context
    - each model only applies within a limited context, clearly define it
    - enforce usage (e.g. in code, database schemas, etc.) to only be within the boundary
    - keeps model internally consistent, without caring about external context
- Continuous integration
    - constantly merge changes with automatic tests to reveal fragmentation of the model within a bounded context
- Context Map
    - when something in bounded context A needs to use functionality in bounded context B
    - context map serves as the translation layer between 2 bounded contexts, translating from B's model to A's model
- Shared Kernel
    - alternative way of sharing functionality between bounded contexts
    - agreed-upon shared subset of both models
- Customer/Supplier
    - when 2 teams own 2 different components, where one is the upstream of the other
    - establish clear responsibilities and requirements
    - jointly develop automated acceptance tests for the upstream component
- Conformist
    - when there's a customer/supplier relationship, but the upstream team has no incentive to cooperate
    - downstream team needs to _conform_ to the upstream's model
- Anti-Corruption layer
    - translation layer between a bounded context and a legacy system
    - ensures our bounded context does not get polluted by models (or lack thereof) from the legacy system
    - uses facades and adapters
- Separate Ways
    - **Integration is always expensive. Sometimes the benefit is small**
    - where possible, avoid having any dependency outside the bounded context
- Open Host Service / Published Language
    - Protocol that opens up access to a subsystem
    - clients can access it as a set of `Service`s
    
> The Trade-Off: More ease of changing relevant code <=> More communication overhead with other teams required

## Distillation
- identify a **Core Domain**: the true business asset the product delivers
- separate the rest out 
    - into their own subdomains
    - use intention revealing interfaces to interact with them
- use abstraction layers within the core to further simplify

## Large Scale Structure
- design should not restrict development too much, rather, it should evolve with the implementation
- organise objects into layers with their own broad responsibilities
- knowledge level pattern: split into 2 levels
    - one for the concrete implementation objects
    - one defining the rules of how those objects should behave (the knowledge layer)
- use abstract core + pluggable components (with specified rules)

## Bringing the Strategy Together
- assess the situation
    - can you draw a good context map or are there still unknowns
    - is there a ubiquitous language
    - is the core domain identified
    - does the technology support model driven design
    - do the devs have the required technical and domain knowledge
- decision making
    - decisions must be communicated to the whole team
    - the plan must take feedback and evolve
    - stay minimalist
    - objects should have a single responsibility, but devs should not be treated the same way
    - don't write frameworks for dummies

---
Source: https://www.goodreads.com/book/show/179133.Domain_Driven_Design
