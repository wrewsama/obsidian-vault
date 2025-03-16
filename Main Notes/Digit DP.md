Tags:
- [[Data Structures and Algorithms]]
---
In this example, we count all numbers till n where the digit state `state` satisfies some condition `cond()`

dp parameters:
- `i`: pointer to a digit position
- `tight`: whether this digit is tightly bounded by `n[i]` (else we can go from 0 to 9)
- `leadingZero`: whether this digit is preceded by leading zeroes. Generally, leading zeroes don't count as part of the state
- `state`: current state of the digits we have so far. Different for each question. Some possibilities include:
    - digit sum
    - digit product
    - last digit picked
```python
def cnt(n: str):
    @cache
    def dp(i, tight, leadingZero, state):
        if cond(state):
            return 1
        res = 0
        maxDigit = int(n[i]) if tight else 9
        for d in range(maxDigit + 1):
            nxtTight = tight and d == maxDigit
            nxtLeadingZero = leadingZero and d == 0
            if nxtLeadingZero:
                # in most cases, leading zeroes shouldn't change our state
                res += dp(i+1, nxtTight, nxtLeadingZero, state) 
            else:
                newstate = transition(state)
                res += dp(i+1, nxtTight, nxtLeadingZero, newstate)
        return res
```
---
## References
- 