## Main features and advantages
- Garbage collection
- Speed
- Concurrency

## Goroutines and channels
Goroutines
- functions executing concurrently in the same address space
- multiplexed onto multiple OS threads by the go scheduler
	- when go program is started, runtime inits n OS threads where n = num physical cores * num threads that can run on each core (i.e max num of OS threads)
	- each OS thread has a Local Run Queue of goroutines
	- goroutines are first sent to the Global Run Queue, scheduler periodically scans GRQ and dispatches goroutines to LRQs
	- OS  thread can steal from other LRQs or GRQ once its own LRQ is empty
	
advantages of goroutines:
- smaller mem footprint
- goroutines have a growable stack space that starts at 2kb, as opposed to OS threads fixed size stack space of 2mb
- faster creation, destruction, context switches

Channels
- allows for communication between goroutines
- also provides synchronisation between reads and writes

## Error handling in Go
error: built in interface type with the Error() string method
- return errors from functions instead of try/catch/throw
- simplifies control flow

## Defer
- executed when surrounding function exits
- executed in LIFO manner

## Panic/Recover
panic: to raise runtime errors in exceptional situations
recover: capture and handle panics gracefully

## Context
Mechanism to control lifecycle, cancellation, and propagation of requests acorss multiple goroutines

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

## Garbage Collection
- most memory is allocated on the stack
- Go uses escape analysis to see if an object's lifetime needs to exceed the stack frames' then allocates it to the heap if so

GC impl: non generational, concurrent, tricolor mark&sweep

(non generational bc the assumption that short-lived objs are reclaimed most often doesn't hold bc go would have allocated them to the stack)

steps:
1. stop the world
2. turn on write barrier on heap (allow goroutines to run // w the GC)
3. resume the world
4. begin tricolor mark&sweep algo
	1. all objs are white
	2. scan stacks, global vars & heap pointers to find used objects and mark them as grey, stop goroutine when scanning stack
	3. for each grey obj o, mark their references as grey, then mark o as black, continue until no more grey objs
	4. stop the world
	5. clean up all white nodes
	6. resume

white: candidates for deletion
black: no references to white objects, cannot be deleted
grey: reachable from roots but not scanned for their references yet, also cannot be deleted, will turn black when scanned

the intermediate grey state is needed to allow GC to run without stopping the world
if a new object is created that's reachable from the root, it needs to be grey so we know we need to scan its references (so we don't wrongly delete whatever it's referencing)