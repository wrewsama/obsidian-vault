Tags:
- [[Concurrency]]
---
## what
- algorithm for deadlock avoidance
- looks at resource requests and ensures that if the request is fulfilled, there exists a way to let all processes finish execution

## intuition
- 3 matrices
    - maximum amount of each resource each process needs
    - amount of each resource already allocated to each process
    - amount of each resource currently available
- algorithm
    - check if the requested resource are even available, if not reject
    - find the amount of each resource each process still needs (max - allocated)
    - simulate and see if there is a way to let every process complete
        - greedily find any process where need <= available
        - "execute" it and free its allocated resources
        - continue until you get stuck (reject) or finish all processes (approve)
---
## References
- https://medium.com/@ahmettemelkundupoglu/understanding-bankers-algorithm-and-deadlock-avoidance-a-comprehensive-guide-97bb0de91198