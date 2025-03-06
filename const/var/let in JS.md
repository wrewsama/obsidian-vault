Tags:
- [[Tags/JavaScript|JavaScript]]
---
const is constant (duh) and block scoped

`let` is block scoped while `var` is globally or function scoped

example:
```javascript
var x = 'old'
let y = 'old'
if (true) {
	var x = 'new';
	let y = 'new';
}
console.log(x); // 'new'
console.log(y); // 'old'
```

---
## References
- 