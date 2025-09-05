Tags:
- [[Distributed Systems]]
---

## Diagram
```
         ┌───────┐      
         │ users │      
         └───┬───┘      
             │          
             │tasks     
             │          
         ┌───▼──────┐   
┌────────┼slurmctld │   
│        └─┬──────┬─┘   
│job       │      │     
│data      │ tasks│     
│    ┌─────▼──┐┌──▼────┐
│    │ slurmd ││slurmd │
│    └────────┘└───────┘
│        ┌──────────┐   
└───────►│ slurmdbd │   
         └────┬─────┘   
              │         
          ┌───▼───┐     
          │sql db │     
          └───────┘     
```

## Components
- `slurmctld`
    - central control daemon
    - high availability mode: has hot standbys for failover
- `slurmd`
    - worker daemons 
    - usually 1 per physical host
- `slurmdbd`
    - optional but common
    - tracks job accounting info (e.g. resource usage, run times)
    - saves that info into a SQL db (e.g. MySQL)
    - sometimes run on the **management node** (the node running `slurmctld`)

note: these are daemon processes, not physical hosts

---
## References
- 