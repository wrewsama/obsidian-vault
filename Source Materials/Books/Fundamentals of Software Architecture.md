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
---
Source: https://www.goodreads.com/book/show/44144493-fundamentals-of-software-architecture
