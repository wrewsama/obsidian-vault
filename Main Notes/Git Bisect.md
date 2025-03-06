Tags:
- [[Tags/Git|Git]]
---
Used to binary search commits to find where the bug first occurred

**Workflow**
1. start session with `git bisect start`
2. Identify bad commit with `git bisect bad`
3. Identify 1st good commit with `git bisect good <good_commit_id>`
4. Git will automatically checkout a commit halfway between the known good and bad commits. If the bug exists, run `git bisect bad`, if not, run `git bisect good`
5. Repeat until you have found the offending commit
6. End the session with `git bisect reset`


---
## References
- 