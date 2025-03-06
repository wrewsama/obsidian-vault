Tags:
- [[Tags/JavaScript|JavaScript]]
---
Function that refers to a var in the env it was created.
example:

```javascript
const add = (function () {

  let counter = 0;

  return function () {counter += 1; return counter}

})();

```

---
## References
- 