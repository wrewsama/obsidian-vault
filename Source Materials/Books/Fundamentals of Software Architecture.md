Tags:
- [[Software Design]]
- [[System Design]]
---
> **First Law of Software Architecture**: Everything in software architecture is a trade-off
> **Second Law of Software Architecture**: _Why_ is more important than _How_

## Modularity
- _module_: logical grouping of related code
- modularity metrics
    - cohesion: LCOM - measures the number of groups of methods that don't use the same fields
    - coupling: 
        - abstractness: `num abstract elements` / `num concrete elements`
        - instability: `efferent (outgoing) coupling / (efferent + afferent (incoming) coupling)`
        - distance from the main sequence: $|abstractness - instability -1|$
            - smaller is better (closer to the "sweet spot")
        - connascence: a change in 1 component requires change in another to maintain correctness of the system

## Architecture Characteristics
- another term for non-functional requirements
- characteristics
    - specifies non-domain design consideration
        - domain: _what_ the app should do
        - non-domain: _how_ the app should do it
    - influences the design of the code structure
    - important to app success
> Never shoot for the _best_ architecture, but rather the _least worst_ architecture
- explicit characteristics
    - taken from the requirements specification
    - e.g. derived from predicted metrics, functional requirements
- implicit characteristics
    - not mentioned / derived from the requirements docs, but still needed
    - probably based on the architect's judgement

## Governance of Architecture Characteristics
- define fitness functions for desired architecture characteristics
- enforce with
    - CI tests (e.g. unit test to check dependencies - enforce _modularity_)
    - External systems (e.g. Netflix's Simian Army)

## Architecture Quanta
- the minimum "unit" in an architecture. Should be:
    - independently deployable: can function even when deployed alone
    - have high functional cohesion: singular obvious purpose
    - have synchronous connascence: everything in the system communicates synchronously
        - can't have extreme differences between each other
        - e.g. if a caller scales up, callee may need to scale up to avoid being overwhelmed
- each quanta will have its own architecture characteristics

## Component-Based Thinking
- Component: a physical packaging of a module
- examples
    - jar files
    - libraries
    - services
- ways to partition into components
    - technical partitioning: by technical capabilities (e.g. presentation, business rules, persistence, etc.), similar to layered architecture
    - domain partitioning: by decoupled workflows (domain driven design)
- component identification cycle
    - identify initial components
    - assign requirements to them
    - analyse responsibilities and architecture characteristics
    - restructure components and repeat as needed
- ways to identify components
    - actors and actions
    - event storming (determine events and design handlers around them)
    - workflows

## Foundations of Architecture Styles
- aka architecture patterns
- a named relationship of components
- fundamental patterns
    - Big Ball of Mud: Anti-pattern, no real structure
    - Unitary: everything running on a single host
    - Client/Server
- Issues with Distributed Architectures
    - unreliable network
    - insecure network
    - nonzero latency
    - finite bandwidth
    - changing network topology
    - multiple network admins
    - nonzero network costs (extra hardware)
    - non-homogeneous networks (mix of multiple hardware vendors)
- other difficulties with distributed architectures
    - distributed logging
    - distributed transactions
    - contract (between client and server) maintenance and versioning

## Layered Architecture
- monolith
- organised into horizontal layers, e.g.
    - presentation
    - business
    - persistence
    - database
- each layer can be
    - open: requests from upper layers can "skip" this layer and directly request lower layers
    - closed: requests from upper layers cannot call lower layers without going through this one
- simple and cheap
- suffers in agility and scalability

## Pipeline Architecture
- monolith
- data flows from _filter_ to _filter_ through _pipes_
- filters can be
    - producers
    - transformers/map
    - testers/reduce
    - consumers (termination point)
- characteristics similar to layered architecture, but with better modularity

## Plugin Architecture
- monolith
- a central core system with independent plug-in components kept in a registry
- all users interact with the core system
- the core system then invokes the plug-ins as required

## Service Based Architecture
- distributed
- partitioned into ~4-12 coarse-grained services within the app
- separation is based on business domains, not technical considerations (e.g. persistence, presentation, etc.)
- naturally, fits well with domain-driven design

## Event Driven Architecture
- distributed
- broker topology
    - data flows through message brokers from event processor to event processor
    - as multiple processors can consumer the same queue, it's more like a graph than a linked list
- mediator topology
    - initial event goes through a queue and is consumed by a mediator
    - the mediator:
        - sends events to different queues for different event processors to pick up
        - waits on callbacks from the event processors, and resume sending the "later" events
        - handles errors
    - there exist open source mediators e.g. Apache Camel, jBPM, etc.

## Space-Based Architecture
- distributed
- rationale: app layer is easier to scale than the DB layer
- app servers interact with data in-memory
    - if data is not in memory, then check database
- that data is 
    - replicated between the servers
    - sent to a message queue to a data writer to be persisted in a database asynchronously
- useful for systems with high traffic spikes that would overwhelm a database

## Microservices Architecture
- distributed (obviously)
- partitioned into business domains, but each domain service contains all necessary parts to operate alone
    - e.g. databases
- sidecar pattern can be used to still have operational reuse
    - e.g. monitoring, logging, service mesh
- microfrontends: split into UI components that interact with only the downstream services it requires
- **Avoid transactions across microservices**. If you need to, it's probably better to combine those services into one

## Choosing Architecture Style
- factors
    - nature of the business domain
    - architecture characteristics
    - existing data architecture (if any)
    - high-level organisational factors
    - operational concerns
- main decisions
    - monolith vs distributed
    - choice of database(s)
    - sync or async communication between services

## Communicating Architecture Decisions
- antipatterns
    - covering your assets
        - avoiding decisions for fear of making the wrong choice
        - solution: continuously collaborate with dev teams to rectify "wrong choices"
    - groundhog day
        - when no-one knows why a decision was made so it gets discussed over and over
        - solution: document the business justification for each major decision
    - email-driven architecture
        - decisions are only mentioned in emails, which are easily lost
        - solution: document in a central repository. When sending emails, send a link to that document
- Architecture Decision Records
    - can be used to document architecture decisions
    - contains: title, status, context, decision, consequences
## Analysing Architecture Risk
- Risk matrix
    - rate likelihood of risk from 1-3
    - rate impact of risk from 1-3
    - risk rating = likelihood * impact
- for each domain area x architecture criteria, assign a risk rating
- risk storming: collaborative risk assessment

## Diagramming and Presenting Architecture
- Representational Consistency: always show the relationship between parts of the architecture before changing views
- If your slides are going to hold all the info, consider sending them as an infodeck instead of giving a presentation
- use a black blank slide to refocus attention on the presenter

## Making Teams Effective
- balance your level of control over the team
- the balance is determined by the:
    - familiarity, size, and experience level of the team
    - complexity and duration of the project
- make use of checklists to ensure certain requirements are met
- provide guidance using business justifications

## Negotiation and Leadership Skills
> The most important single ingredient in the formula of success is knowing how to get along with people
- avoid coming across as argumentative
- justify your choices in terms of cost and time
- demonstration defeats discussion

## Developing a Career Path
- 20 minute rule: devote at least 20 minutes a day to 
    - deep dive into something specific
    - learn something new
- practice makes perfect, do some [katas](https://fundamentalsofsoftwarearchitecture.com/katas/)
---
Source: https://www.goodreads.com/book/show/44144493-fundamentals-of-software-architecture
