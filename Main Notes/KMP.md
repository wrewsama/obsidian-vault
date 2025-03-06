Tags:
- [[Tags/Data Structures and Algorithms|Data Structures and Algorithms]]

---
Idea:
For each index `i` in the pattern, consider the subarray ending at `i`
Let `lps[i]` be the longest prefix that is also a suffix of that subarray

Now let `p1` be a pointer to the main string and `p2` be a pointer to our pattern.
When we get a mismatch, we now only need to make `p2` jump back to `lps[p2-1]`, because we know that `pattern[:that index]` will be the same as whatever we matched earlier in the main string (cause that's a suffix of `pattern[:p2]`.
Hence, we never have to move p1 backwards so time complexity = `O(len(string) + len(pattern))`

Code:
```python
def strStr(self, haystack: str, needle: str) -> int:
	lps = [0 for _ in range(len(needle))]
	ptr, prevlps = 1, 0
	while ptr < len(needle):
		if needle[ptr] == needle[prevlps]:
			prevlps += 1
			lps[ptr] = prevlps
			ptr += 1
		elif prevlps == 0:
			lps[ptr] = prevlps
			ptr += 1
		else:
			prevlps = lps[prevlps-1]
	
	ph, pn = 0, 0
	while ph < len(haystack):
		if needle[pn] == haystack[ph]:
			ph += 1
			pn += 1
		elif pn == 0:
			ph += 1
		else:
			pn = lps[pn-1]

		if pn == len(needle):
			return ph - len(needle)

	return -1
```

The first part can be extended to find the longest prefix in `word` that's a suffix in `target`
Technique (replace the $ with any character that's guaranteed to not appear in target): 
```python
genlps(word + '$' + target)
```

---
## References
- 