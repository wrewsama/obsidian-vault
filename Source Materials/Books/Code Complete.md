Tags:
- [[Code Quality]]
- [[Software Engineering]]
---
## Software Development Metaphors
- metaphors model the development process, 
- they provide a heuristic to guide your choices
- some common metaphors
    - software development: constructing a house
    - software techniques: toolbox

## Prerequisites
- project phases
    - **prerequisites**
    - construction
    - testing
- 2 main prerequisites
    - requirement gathering
    - architecture design
- significance
    - avoid cost of building the wrong thing
    - useful even when doing iterative development
- requirements checklist
    - functional requirements
        - tasks
            - inputs
            - outputs
        - external interfaces
    - non-functional requirements
        - latency
        - security
        - reliability
        - resource usage
        - maintainabilty: ability to adapt to changes in functionality, external interfaces, environment, etc.
    - quality
        - any conflicts
        - consistent level of detail
        - clarity
        - relevance
        - testability
        - completeness: if all requirements are satisfied, is product "done"
- architecture
    - system-wide design overview
    - major classes (subsystems)
    - data design (data structures, schemas)
    - UI design
    - resource usage estimates / resource management solutions
    - estimates & how to handle
        - security
        - performance
        - scalability
        - interoperability
        - i18n
    - error handling
    - feasibility
    - change strategy (most likely changes should be easy to implement)
> **Conceptual Integrity**: You should be please by how natural and easy the solution seems. It shouldn't look as if the problem and the architecture have been forced together with duct tape.

---
Source: https://www.goodreads.com/book/show/4845.Code_Complete
