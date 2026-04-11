Tags:
- [[Operating Systems]]
- [[Linux]]
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

## Linux Completely Fair Scheduler (CFS)
- each process accumulates `vruntime`
- when scheduling decisions occur, pick the process with the lowest `vruntime`
	- this is done using a red black tree
- scheduling decisions occur after a certain latency. This latency is proportional to `1 / num_processes` (as more processes start, latency decreases to some fixed floor)
- _niceness_
	- each process has a nice level between $-20$ and $19$ (default = $0$)
	- each nice level is mapped to a `weight`, higher niceness => lower weight
	- when process $i$ runs, increment $vruntime_i$ by $\frac{weight_0}{weight_i} * runtime_i$
- when new processes start or when sleeping processes wake up, their `vruntime` is set to the lowest `vruntime` in the red black tree, not 0 (prevent it from hogging CPU)
---
## References
- [[Operating Systems Three Easy Pieces]]