Tags:
- [[Tags/Databases|Databases]]
---
**Dirty Read**
Read uncommitted data, then the writer transaction rolls back

**Non-repeatable Read**
Read, other transaction writes, read again => different value on same row

**Phantom Read**
Query on some condition, other transaction inserts new rows that also satisfy that condition, reading again gives different set of rows

**Write Skew**
2 transactions read common data, make decisions based on that data, then update records within that shared data and possibly violate a state constraint. The records need not be the same ones.

Example:
state constraint: sum of some field A must be >0
1 transaction does:
```
if select sum(A) from T > 0:
	update T set A = A - 1 where B = 'something'
```
and the other does:
```
if select sum(A) from T > 0:
	update T set A = A - 1 where B = 'something else'
```

**Lost Update**
2 transactions read the same data and try to update, 1st update gets overwritten by 2nd
Special case of write skew where the transactions update the same row
---
## References
- 