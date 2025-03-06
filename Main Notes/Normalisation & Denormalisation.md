Tags:
- [[Tags/Databases|Databases]]
---
Normalisation: Process of removing redundant data from the database by splitting the table in a well defined manner to maintain data integrity

Denormalisation: Process of adding redundant data in the table to speed up complex queries
- "pre-joins" related tuples which can reduce the amount of IO for common workload patterns
- harder to modify that redundant data
	- e.g. changing the faculty of a major => every student taking that major needs its faculty field updated too to maintain data consistency

1NF: All entities contain only unique or atomic values

2NF: 1NF AND all non key attributes are fully dependent on the _entire_ primary key

3NF:
- every non prime attribute depends on the entire key and only the key

BCNF:
- every single attribute depends on only the key
- difference from 3NF: No multiple overlapping candidate keys

4NF:
- multivalued dependencies in a table must be multivalued dependencies on the key
	- multivalue deps: X -> one of the elements in a particular subset of Y 
	- written as X ->> Y

5NF:
- 4NF and cannot be describable as the logical result of joining some other tables together
---
## References
- 