Tags:
- [[Operating Systems]]
---
Criteria:
- fairness
- utilisation

metrics:
- waiting time (arrival to 1st share of cpu)
- turnaround time (arrival to end)
- throughput (total tasks / unit time)

examples:
priority, first come first serve, round robin, MLFQ

**MLFQ**
adaptive scheduling algo

Basic Rules:
- If prio(A) > Prio(B), A runs first
- If prios are the same, they run in RR

Prio setting/changing rules
- new job -> highest prio
- Job fully utilise time quantum -> reduce prio
- Job gives up / blocks before finishing time quantum -> prio retained

---
## References
- 