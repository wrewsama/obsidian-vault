Tags:
- [[Data Structures and Algorithms]]
---
note: starts from chapter 5 since this is a continuation of [[CP4 Book 1]]
## Chapter 5: Math
- number of digits in `x` = `floor(1+log(x, 10))`
- expressing fractions: divide both numerator and denominator by their gcd
- Check if a number is prime: check divisors up till $\sqrt{N}$
- Generate list of prime numbers: Sieve of Eratosthenes
- Prime factorisation: sieve, then recursively look for and divide by the first prime factor stopping at $\sqrt{N}$ because the only way there's a prime factor greater than that is if $N$ is prime itself, so handle that separately
- Generate Fibonacci numbers: O(n) dp
- Generate nth Fibonacci number: O(log n) $(\phi^n - (-\phi)^{-n}) / \sqrt{5}$
- $_nC_k$: `comb`, $\frac{n!}{k!(n-k)!}$, or Pascal's triangle
- Catalan numbers: $Cat(n) = \frac{_{2n}C_n}{n+1}$, where `Cat(0) = 1`
- Finding cycle in linked list: Floyd's Cycle-Finding Algorithm
- Game theory: Minimax
- Number of paths of length L: adjacency matrix ^ L
- modular exponentiation: `pow(x, y, z)` = $x^y mod(z)$ complexity O(logy)
- efficient matrix exponentiation: same principle as the fast exponentiation: if odd power, `b*exp(b, n-1)`, if even, `exp(b, n//2)**2`. $O(n^3log(n))$

## Chapter 6: String Processing
- String alignment / Edit distance: 2D dp
- Longest common subsequence: 2D dp
- String Matching: [[KMP]]
- String Matching in 2D grid: recursive backtracking
- Longest repeated substring: suffix trie/tree/array
- String Matching: Rabin Karp with [[String Rolling Hash]]
- Anagrams: sorting or hashing frequencies
- Check Palindrome: O(n) 2 pointers
- count palindrome substrings: O(n^2) dp - check if each `start, end` is a palindrome by recursing on start or end - 1 and memoise the results

## Chapter 7: Geometry
- dot product: `p1.x * p2.x + p1.y * p2.y` 
- cross product (`p1 x p2`): `p1.x * p2.y - p1.y * p2.x`
- Angle $AOB$ given 3 points A, O and B: Use $|OA| \cdot |OB| = |OA||OB|cos(\theta)$
* CCW test: check if p->q->r forms a counter-clockwise turn: check $pq \times pr > 0$
- distance from point $p$ to line segment $ab$: 
    - find multiplier $u$ such that $u*ab$ = projection of $ap$ onto $ab$: $u = \frac{ap \cdot ab}{|ab|^2}$
    - if $u < 0$: $p$ is "behind" a, return distance between p and a
    - if $u > 1$: $p$ is "in front of" b, return distance between p and b
    - else, need the perpendicular distance from p to ab, return the Euclidean distance from $a + u*ab$ to $p$
- area of polygon: **Shoelace Formula**: for all `i` except the last, `res += p[i].x * p[i+1].y - p[i+1].x * p[i].y`, then `res = 2*abs(res)`
- checking if polygon is convex: use CCW test
- checking if point p is inside polygon: for each pair of points p1, p2 on the polygon, get the angle between p1, p, and p2 then add it to res if it's counter-clockwise else subtract it from res. return `res == 2 pi`
- Convex hull: Andrew's Monotone Chain

---
Source: https://www.goodreads.com/book/show/54698024-competitive-programming-4---book-2
