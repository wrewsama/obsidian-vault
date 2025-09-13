Tags:
- [[Software Engineering]]
---
## What is SWE
> The application of an empirical, scientific approach to finding efficient, economic solutions to practical problems in software
- SWE is more about designing solutions than producing them
- Precision and scalability > craftsmanship
- All engineering is about optimisation and trade-offs
- Foundations
    - Learning
    - Managing Complexity

## Optimising for Learning
- Iteration: repeating a sequence of operations to yield results closer to what's desired
    - need to make small changes that can be tested out (and removed) with minimal cost
- Feedback
    - provides evidence for a decision
    - the earlier the better
    - feedback for:
        - coding: tests, IDE tools
        - integration: CI
        - design (object-level): TDD can help with this
        - architecture (service-level): CD, generally with microservices
- incrementalism
    - modularity: isolated components that can be developed independently of one another
- empiricism
    - use observable evidence over theoretical reasoning or intuition
    - assume that what you think is wrong, then try to find out _how_ it's wrong
    - can use automated tests as your experiment

## Optimise for Managing Complexity
- Modularity: how easily a system's components can be split and put back together
    - modularity improves testability
    - designing for testability also improves modularity
- Cohesion: within a module, how closely related are the constituents
    - Put simply, if you want to change 1 part,
        - high cohesion => the things that need to be changed are all close together
        - low cohesion => the things that need to be changed are all over the place and hard to find

---
Source: https://www.goodreads.com/book/show/57345270-modern-software-engineering
