Tags:
- [[Math]]
- [[Data Structures and Algorithms]]
---
## what
- iterative way to approximate the root of $f(x) = 0$ 
- formula: $x_{n+1} = x_n - \frac{f(x_n)}{f'(x_n)}$, where $x_i$ is your $i$th guess, with each guess getting closer to the root

## intuition
- draw a tangent on the graph of $y=f(x)$ at $x_n$
- set $x_{n+1}$ to the x-intercept of that tangent line

## fast square root
- efficient algorithm to obtain the square root
- apply the formula to $f(x) = x^2 -a$
- $x_{n+1} = 0.5 * (x_n + \frac{a}{x_n})$
- achieves **quadratic convergence**: i.e. errors get squared on each step (massive decrease for error < 1)
    - even faster than binary search, which halves the errors on each step

---
## References
- https://math.mit.edu/~stevenj/18.335/newton-sqrt.pdf