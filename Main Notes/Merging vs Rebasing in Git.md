Tags:
- [[Tags/Git|Git]]
---

| Merging                                        | Rebasing                                      |
| ---------------------------------------------- | --------------------------------------------- |
| Preserves complete history with merge commits  | Creates a linear history by rewriting commits |
| Resolves all merge conflicts in 1 merge commit | Need to conflicts commit by commit            |
Golden rule: never rebase a public branch (e.g. main / master or something other people are using) because your history will diverge from others'


---
## References
- 