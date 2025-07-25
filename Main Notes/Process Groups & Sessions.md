Tags:
- [[Operating Systems]]
- [[Linux]]
---
- 2 Level hierarchy used to organise processes
    - process groups have multiple processes, identified by a PGID
    - sessions have multiple process groups, identified by a SID
- PGID = PID of process group leader
- can send signals (e.g. `SIGINT`, `SIGTERM`) to entire process group easily
- subprocesses inherit the parents' PGID when spawned unless explicitly changed
- processes run in a pipeline also use the same process group

---
## References
- https://biriukov.dev/docs/fd-pipe-session-terminal/3-process-groups-jobs-and-sessions/
- https://www.elastic.co/blog/linux-process-and-session-model-as-part-of-security-alerting-and-monitoring
- https://stackoverflow.com/questions/41498383/what-do-the-identifiers-pid-ppid-sid-pgid-uid-euid-mean