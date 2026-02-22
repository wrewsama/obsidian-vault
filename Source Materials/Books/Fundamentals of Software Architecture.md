Tags:
- [[Software Design]]
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
---
Source: https://www.goodreads.com/book/show/44144493-fundamentals-of-software-architecture
