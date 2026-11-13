Tags:
- [[Software Engineering]]
---
- lambda calculus
    - rules
        - only functions, no data/types
        - no state/side effects
        - all functions are unary (use currying for multi-argument functions)
    - notation: $(\lambda x.x + 1): v -> v$
        -  equivalent to 
        - ```ocaml
        fun (x: int): int -> x + 1
          ```
    - $\alpha$ conversion: renaming variables without changing behaviour
    - $\beta$ reduction: replace bound variables with arguments
- special types
    - monoid: group with a binary operation that:
        - always returns an object from the group (i.e. has the form `'a * 'a -> 'a`)
        - associative (but not necessarily commutative)
        - has an identity element that makes the operation do nothing
    - semigroup: same restrictions as monoid except no identity element

---
Source: https://www.goodreads.com/book/show/43517730-functional-programming-for-dummies
