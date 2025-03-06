Tags:
- [[Tags/Data Structures and Algorithms|Data Structures and Algorithms]]
---

for fast string/substring comparisons

intuition:
`hash(s)` = sum of `s[i] * p**i` mod `m`, for all `i in range(len(s))` 
`p` is a prime number roughly equal to the num of chars in the input alphabet
`m` should be a large number

after precalculating all the hashes starting from 0,
`hash(s[i:j]) * p**i` = `hash(s[0:j]) - hash(s[0:i]) mod m`

hence , let `n = len(s)`, 
`hash(s[i:j]) * p**n = p**(n-i) * (hash(s[0:j]) - hash(s[0:i])) mod m`
so for each substring i:j, we actually compare the hash * p^n

code:
```python
p = 31
m = 10**9 + 9
hashes, pows = [0 for _ in range(len(s) + 1)], [1 for _ in range(len(s) + 1)]

for i in range(len(s)):
	pows[i+1] = pows[i] * p % m
for i in range(len(s)):
	hashes[i+1] = (hashes[i] + (ord(s[i]) - ord('a') + 1) * pows[i]) % m

def getHash(start, end):
	h = (hashes[end] + m - hashes[start]) % m
	return h * pows[len(s)-start] % m
```

---
## References
- 