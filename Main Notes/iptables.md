Tags:
- [[Linux]]
- [[Computer Networking]]
---
## what
- Linux tool to set rules to inspect, modify, forward, redirect, or drop IP packets
- contains tables, that contain several chains, that have rules, each with a condition and an action to perform if a packet matches that condition
## important concepts
- tables
    - `filter`: the "default" table
    - `nat`: network address translation rules
    - `raw`, `mangle`, `security`: other tables, not commonly used
- chains (different tables contain different chains)
    - `INPUT`
    - `OUTPUT`
    - `FORWARD`
    - `PREROUTING`
    - `POSTROUTING`
- rules: condition + action
    - condition include src/dest IP, protocol, port number 
    - actions: `ACCEPT, DROP, REJECT, QUEUE, RETURN`

## basic commands
- add rule: `iptables [-t table] --append [chain] [parameters]`
- delete rule: `iptables [-t table] --delete [chain] [rule_number]`
- list tables: `iptables --list`

---
## References
- https://wiki.archlinux.org/title/Iptables
- https://www.geeksforgeeks.org/linux-unix/iptables-command-in-linux-with-examples/