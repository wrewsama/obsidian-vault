Tags:
- [[Data Structures and Algorithms]]
- [[Math]]
---
## congruence notation
$a \equiv b \pmod{m}$
=> `a % m == b % m`

## addition/subtraction
`(a + b) % m == ((a % m) + (b % m) % m)`
`(a - b) % m == ((a % m) - (b % m) % m)`

## multiplication
`(a * b) % m == ((a % m) * (b % m) % m)`

## exponentiation
`(a**k) % m == ((a % m)**k % m)`
(in python, `pow(a, k, m)`)

## multiplicative inverse
- modulo isn't distributive for division
- need to multiply a _modular multiplicative inverse_
- defined as $x$ such that $a * x \equiv 1 \pmod{m}$
- can use [[Fermats Little Theorem]] to find
---
## References
- https://brilliant.org/wiki/modular-arithmetic/