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

---
Source: https://www.goodreads.com/book/show/179133.Domain_Driven_Design
