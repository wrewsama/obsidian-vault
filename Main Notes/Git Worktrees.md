Tags:
- [[Git]]
---
## what
- git feature allowing you to work on multiple branches at the same time
- creates a new folder checking out a branch
- underlying object DB still points to the main worktree folder => more efficient than just cloning your repo multiple times

## how to use
- create
    - `git worktree add ../some_folder existing_branch`
    - `git worktree add ../some_folder -b new_branch`
- list
    - `git worktree list`
- remove
    - `git worktree remove ../some_folder`

---
## References
- https://youtu.be/S8_AsOuAwLo?si=Po4H7WSCkyecpBf6
- https://www.datacamp.com/fr/tutorial/git-worktree-tutorial