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
---
Source: https://www.goodreads.com/book/show/44144493-fundamentals-of-software-architecture
