Tags:
- [[Data Structures and Algorithms]]
- [[Math]]
---
## the theorem
- let $p$ be a prime number, and $a$ be any integer
- $a^p - a$ is always divisible by $p$
- => $a^p \equiv a \pmod{p}$

## special case for modular multiplicative inverse
- let $p$ be a prime number, and $a$ be any integer, **AND $a$ is not divisible by $p$**
-  $a^{p-1} \equiv 1 \pmod{p}$
- => $a * a^{p-2} \equiv 1 \pmod{p}$
- => modular multiplicative inverse = $a^{p-2}$

```python
p = 10**9 + 7 # prime number
x, y = 420, 69 # any numbers
val = x * y % p
x1 = val * (pow(y, p-2, p)) % p
assert x == x1
```
---
## References
- https://brilliant.org/wiki/fermats-little-theorem/
- https://testbook.com/maths/fermats-little-theorem#:~:text=In%20summary%2C%20we%20have%20shown,This%20is%20Fermat's%20Little%20Theorem.