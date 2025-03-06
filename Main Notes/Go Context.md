Tags:
- [[Tags/Go|Go]]
---
Mechanism to control lifecycle, cancellation, and propagation of requests across multiple goroutines

**Timeout:**
set timeout with context's `WithTimeout` method
receive from `ctx.Done()` channel (`ctx` writes to this `chan` when timeout reached)

**Save/Retrieve Vals:**
essentially a `map[string]any`
save: use `context.WithValue`
retrieve: `ctx.Value(key string)`

**Cancel:**
`context.WithCancel` returns the wrapped context and a cancel function
calling the cancel function writes to the `ctx.Done()` channel


---
## References
- 