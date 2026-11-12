Tags:
- [[SRE]]
---
- **Generic Mitigation**: something that can stop/reduce the impact of a wide range of different issues
- important characteristic of generic mitigations: _You don't need to fully understand the outage to use them_
- common patterns
    - code rollback
    - data rollback
    - degradation (degrade the service to decrease load on hosts)
    - upsize (scale out infra)
    - blocklists (block problematic sources)
    - drain (move traffic elsewhere)
    - quarantine (prevent "bad instances" from affecting others)
- ensure the team is familiar with using all the mitigation tools

---
Source: https://www.oreilly.com/content/generic-mitigations/
