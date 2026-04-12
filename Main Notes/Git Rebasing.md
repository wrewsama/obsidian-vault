Tags:
- [[Tags/Git|Git]]
---
rebase current branch onto `other_branch`: `git rebase other_branch`

- find all the commits in the current branch but not in `other_branch`
- apply those commits to the `other_branch` one by one
    - if there is a merge conflict, user needs to resolve it
    - once resolved, `git add .` and `git rebase --continue`
    - if it cannot be resolved, abort with `git rebase --abort`
- updates HEAD of current branch to point to latest commit

---
## References
- https://git-scm.com/docs/git-rebase