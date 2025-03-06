Tags:
- [[Tags/Databases|Databases]]
- [[Redis]]
---
`MULTI/EXEC`:
- When `MULTI` is called, all subsequent redis commands are queued
- all commands are executed atomically once `EXEC` is called
- this means we can't use any intermediate values

Transactions in SQL:
- After using `BEGIN`, can execute operations immediately
- writes get committed only after `COMMIT` (or rolled back if `ROLLBACK`)
- intermediate values can be used in between operations

---
## References
- 