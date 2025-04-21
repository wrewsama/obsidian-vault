Tags:
- [[Operating Systems]]
- [[Concurrency]]
---
Handling deadlocks:
- [[Deadlock Prevention]]: ensuring none of the 4 [[Deadlock Conditions]] are satisfied
- Avoidance: when process requests resource, algo examines the resource allocation state, if that allocation causes the state to be unsafe, reject the request (example: Banker's Algorithm)
- Detection/Recovery: abort (either all processes at once, one by one till deadlock is  resolved, or taking resources from lower priority to higher priority processes one by one till deadlock resolved)
- ignorance: just reboot


shitty analogy: assume getting into a deadlock is like slipping on a banana peel
- prevention: **prevent** that banana peel from being on the floor in the first place (i.e. prevent the _conditions_ from happening)
- avoidance: seeing the banana peel, and **avoiding** it
- detection/recovery: stepping on the banana peel, but moving to **recover** your balance
- ignorance: **ignorantly** stepping on and slipping on the banana peel, then getting back up

---
## References
- [[Operating Systems Three Easy Pieces]]