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

---
Source: https://www.goodreads.com/book/show/179133.Domain_Driven_Design
