Tags:
- [[Tags/Databases|Databases]]
---
![[Pasted image 20241125212631.png]]

**lock-based implementation**
![[Pasted image 20241125212639.png]]

**MVCC Implementation**
Read Committed:
- each query sees version of data committed before the QUERY began
- on an update (or `DELETE` or `SELECT FOR UPDATE`), if the target row is locked by a concurrent transaction, wait
	- other transaction rolls back: we can commit
	- other transaction commits: apply update only the updated version of the row 

Repeatable Read:
- each query sees version of data committed before the TRANSACTION began
- on a write, if the target row is locked by a concurrent transaction, wait
	- other transaction rolls back: we can commit
	- other transaction commits: we roll back and throw error

Serialisable:
- [Serialisable Snapshot Isolation](https://drkp.net/papers/ssi-vldb12.pdf)
	- monitors the rw-dependencies between transactions and aborts as needed
	- tracked and detected using predicate locks

**weird stuff**
repeatable read prevents write skew when using lock-based implementation, but not on MVCC
dirty reads are impossible on postgres, even on read uncommitted
read committed also prevents unrepeatable reads when using MVCC (still counts bc the ANSI SQL standards are a minimum standard that can be exceeded)

---
## References
- https://drkp.net/papers/ssi-vldb12.pdf 