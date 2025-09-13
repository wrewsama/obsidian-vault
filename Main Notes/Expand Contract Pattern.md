Tags:
- [[System Design]]
---
- for implementing backward-incompatible changes to interface or schema
- 3 phase system
    - expand: add new things, letting them coexist with the old
    - migrate: update clients to use new things
    - contract: remove old things from interface/schema and code
---
## References
- 