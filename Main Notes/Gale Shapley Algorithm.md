Tags:
- [[Data Structures and Algorithms]]
---
## why
- solves the **Stable Marriage problem**
- given men and women along with their preferences, find a way to pair them such that it is _stable_
    - _stable_: there is no man $m$ and woman $w$ such that they prefer each other more than their allocated partners

## how
#### data structures
- stack/queue of unmatched men
- mapping of men to a list of women in descending order of preference (index 0 is most preferred)
- mapping of men to their next _proposal rank_ (i.e. which woman in his preference list will he propose to next. Initialise all to 0)
- mapping of women to their ranking of men (i.e. if `women_ranking[w][m]` = n, that means `m` is `w`'s nth preference)
- matches (mapping from woman to man)

#### intuition
- pop a man from the unmatched men collection, call him `m`
- propose to the woman corresponding to this man's preferences and current proposal rank
- if she's unmatched, match the 2 and continue
- if she prefers her existing match
    - increment `m`'s proposal rank (intuition: lowering his standards by making him aim for the next woman on his ranking)
    - push `m` back to the unmatched pile
- if she prefers `m`
    - match the woman with `m`
    - increment the dumped man's proposal rank
    - push the dumped guy into the unmatched pile :(
- continue until there are no more unmatched men

#### time & space
$O(n^2)$

---
## References
- https://getcracked.io/problem/125/swap-auction-matching?language=Python
- https://medium.com/aiskunks/understanding-gale-shapley-stable-matching-algorithm-and-its-time-complexity-4b814ee2642