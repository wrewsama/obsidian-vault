## Chapter 2: Intro
purpose of the OS:
- virtualisation
	- CPU
	- memory
- concurrency
- persistence

design goals:
- abstraction
- performance
- isolation between applications
- reliability

# Virtualisation
## Chapter 4: Processes
process: abstraction for a running program
process list / Process Control Block: data structure used by OS to keep track of all processes in the system

processes contain:
- address space
- registers
	- program counter (PC)
	- stack pointer
	- frame pointer
- IO information (e.g. open files)

API:
- create
- destroy 
- wait
- misc control
- status

states:
- running
- ready
- blocked

## Chapter 5: Process API
`fork()`
- creates new process and continues executing the remaining code
- returns
	- for parent: PID of child 
	- for child: 0

`wait()`
- blocks until a child finishes execution

`exec()`
- executes some other program

`open()`
- opens file and assigns the smallest unopened file descriptor to it
- can `close()` STDOUT / STDIN and use open to attach the smallest file descriptor to a given file
- subsequent `exec()` will use those default file descriptors

`kill()`
- sends signals to a process (e.g. SIGINT, SIGTSTP)

## Chapter 6: Limited Direct Execution
time sharing: let each process have the CPU for a little while, then switch to another. Lets multiple processes share physical CPU at the same time

restricted operations:
- 2 modes
	- user mode: runs with restrictions
	- kernel mode: has privileges
- syscalls: allow kernel to expose key functionality to user programs
- process
	0. kernel sets up **trap table** at boot time
		- maps syscall numbers to **trap handlers**
		- essentially, OS tells hardware which code to run
	1. program calls syscall and executes **trap** instruction
	2. hardware
		1. saves registers to kernel stack
		2. jumps to appropriate trap handler
	3. OS executes syscall then the **return-from-trap** instruction
	4. hardware
		1. restores registers from kernel stack
		2. jumps back to PC

switching between processes:
- Cooperative approach: wait for syscalls
- Non-Cooperative approach: timer interrupt
	- at boot time, OS tells hardware which code to run upon timer interrupt 
- context switch process
	0. kernel sets up trap table and starts the interrupt timer
	1. hardware
		1. executes timer interrupt
		2. saves A's user registers to A's kernel stack
		3. jump to handler
	2. OS
		1. save A's kernel registers
		2. restore B's kernel registers
		3. switch to B's kernel stack
		4. return-from-trap
	3. hardware
		1. restores B's user registers from B's kernel stack
		2. jump back to PC

## Chapter 7: Scheduling
metrics:
- turnaround time: completion time - arrival time 
	- i.e. how long it takes to finish each job
- response time: time when the job is run for the first time - arrival time 
	- i.e. how long it takes to start each job
- fairness: e.g. Jain's Fairness Index

## Chapter 8: Multi-Level Feedback Queue
MLFQ approximates Shortest Job First scheduling without a priori knowledge of job lengths. This is to minimise turnaround time (how fast you finish), as well as response time (how fast you begin executing)

rules:
- if Priority(A) > Priority(B), A runs
- if Priority(A) == Priority(B), A & B run in round robin using the given time slice
- new jobs start at the highest priority
- once job uses up its time allotment at a given level (regardless of whether it gives up the CPU), decrement its priority
- after some time period, reset all jobs to the highest priority

## Chapter 9: Proportional Share

lottery scheduling:
- tickets are distributed among processes
- winning ticket is picked every time slice, process with that ticket runs

stride scheduling:
- define $stride_i = \frac{\sum tickets}{tickets_i}$
- when process $i$ runs for a time slice, $pass_i += stride_i$
- keep running the process with the lowest $pass$
- intuitively, more tickets => less stride => can run more

Linux Completely Fair Scheduler (CFS):
- each process accumulates `vruntime`
- when scheduling decisions occur, pick the process with the lowest `vruntime`
	- this is done using a red black tree
- scheduling decisions occur after a certain latency. This latency is proportional to `1 / num_processes` (as more processes start, latency decreases to some fixed floor)
- _niceness_
	- each process has a nice level between $-20$ and $19$ (default = $0$)
	- each nice level is mapped to a `weight`, higher niceness => lower weight
	- when process $i$ runs, increment $vruntime_i$ by $\frac{weight_0}{weight_i} * runtime_i$
- when sleeping processes wake up, their `vruntime` is set to the lowest `vruntime` in the red black tree (prevent it from hogging CPU)

## Chapter 10: Multiprocessor Scheduling
issues of a single queue:
- synchronisation: different cores need concurrent access to the shared scheduler
- cache affinity: jobs that keep switching core will have more cache misses

multi-queue scheduling:
- have a separate scheduling queue for each core, each job is assigned to one of the queues
- work stealing: underused queue can check to see if other queues have more, then reassign those jobs to the underused queue

## Chapter 13: Address Spaces
address space components (from low address to high address):
- text
- data
- heap
- stack
heap and stack grow towards each other

goal of virtual memory
- transparency: let program behave as if it has its own private physical memory
- efficiency: implementation should be fast and use minimal extra data structures
- protection: isolate processes from affecting one another

## Chapter 14: Memory API
types of memory:
- stack: for short lived local variables / params in function calls, managed implicitly by the compiler
- heap: for long-lived variables, explicitly handled by programmer 

##### `malloc()` / `free()` example:
malloc: given size `n`, allocate `n` bytes on the heap and return a pointer to the newly allocated space
free: given a pointer that was previously returned by `malloc`, release the memory region allocated by that malloc call (_note that the memory allocation library tracks the entire region assigned to that pointer_)
```c
#include <stdlib.h>
int *a = (int *) malloc(10 * sizeof(int))
free(a)
```

> important note: `malloc` and `free` are **library calls**, NOT syscalls. The malloc library manages part of the virtual address space by using syscalls 
##### Common Errors
- forgetting to allocate memory => segfault
- not allocating enough memory => buffer overflow
- not initialising allocated memory => uninitialised read
- not freeing memory => memory leak
- freeing memory before you're done with it => dangling pointer
- freeing memory repeatedly => double free
- calling `free()` on a pointer that wasn't `malloc`'ed => invalid free 

##### Other options
- `mmap`: for large, infrequent allocations
- `calloc`: `malloc` but zeroes the memory before returning
- `realloc`: add more space to a `malloc`ed region

## Chapter 15: Address Translation
- translate the _virtual address_ provided by the instruction to a _physical address_
- creates the illusion of private memory for each process
- relocation
	- use 2 registers to store the base address and the bounds (limit)
	- to translate, check if $addr_{virtual} \leq limit$, if so, do $addr_{physical} = addr_{virtual} + base$, else, trap to OS and kill
	- done in the processor's memory management unit (MMU)
	- stored in PCB when context switching away

## Chapter 16: Segmentation
- general idea: keep base and bounds for different segments of a program's address space
- implementations
	- pair of registers for each segment (faster)
	- segment table mapping segment number to base & bounds addresses (supports more _fine-grained_ segments)
- additions:
	- bit to check if the segment grows in the positive direction (heap) or negative (stack)
	- protection bits: for sharing the same segments between different processes
		- the virtual addresses point to the same physical address region

## Chapter 17: Free Space Management
external fragmentation: holes of free space in physical memory between allocated blocks
internal fragmentation: unused part of a memory block allocated to a process/segment 

free list implementation:
- linked list of free spaces
- each node contains
	- start address
	- length
- when allocating memory, list is traversed to find a suitable free space, then that region is allocated and the node is updated
- when freeing memory, new free list node is inserted. Existing nodes are coalesced if needed

allocation strategies:
- best fit: smallest hole that the request fits in
- worst fit: biggest hole
- first fit: first hole that request fits in
- next fit: same logic as first fit, except search continues where the previous allocation left off instead of at the beginning
	- ensures searches are spread out over the list instead of splintering the beginning of the list
- segregated lists: keep separate lists to handle requests of fixed sizes
- buddy allocation: keep blocks with size = some power of 2, then halve / merge them as you allocate and free

## Chapter 18: Paging Intro
- chop up space into fixed sized pieces
- addresses: first part = page number, second part = offset within the page
- page table:
	- one for each process
	- stored in memory
	- maps virtual page number to physical page number
	- also contains
		- valid bit: to mark virtual page numbers that aren't used yet (e.g. the empty space for the heap / stack to grow)
		- protection bits: permissions
		- reference bit: to track popular pages to ensure they don't get swapped out

## Chapter 19: TLB
translation lookaside buffer (TLB):
- caches virtual page number <> physical page number mappings
- part of the MMU in hardware
- when handling a virtual address, the hardware first checks the TLB
	- TLB hit: extract that physical page number and append the offset bits to it
	- on TLB miss:
		- for CISC: handle directly using hardware, save location of page table in a special register
		- for RISC: trap to OS and let the software handle
		- in both cases, need to get the physical page number from the page table in memory, update the TLB, then retry the instruction
- entries consist of:
	- VPN
	- PPN
	- valid bit
	- protection bits

handling context switches:
- translations are only valid for the currently running process
- solutions
	- flush TLB every context switch => many cache misses just after a switch
	- add a address space identifier to each entry

## Chapter 20: Other Page Tables

paging + segmentation hybrid:
- like segmentation, except the base and bounds registers are different
- base: now holds the address of the page table for that _segment_
- bounds: holds the number of page table entries that segment is using
- saves space because we don't need page table entries for the unused region between the heap and stack

multi-level page tables:
- given $2^x$ byte page size, last $x$ bytes in the VPN are the bottom level page table's index, the $x$ bytes before that are the 2nd last level page table, and so on
- the page table itself is split into pages
- each page table above the bottom maps index to the address of the appropriate next level page table
- for a higher level page table entry, if none of the physical pages under it are allocated, don't create any page table for it

inverted page tables:
- instead of a per-process table mapping VPN to PPN, have 1 table mapping PPN to process and VPN

## Chapter 21: Swap Memory
- required to support large address spaces with limited memory
- stash away parts that aren't in great demand

present bit: 
- bit inside the page table entry to indicate if the page is in memory or swap
- on TLB miss:
	- if present, use that PPN
	- if not, raise **page fault**
	
page fault handler:
- if memory is full
	- evict page based on some policy
- read the PTE's page from disk into a free physical memory page
- mark the PTE as present
- change the PTE's PPN value to the page we just loaded to
- retry the instruction (this will cause a TLB miss, which will work after another retry)

when replacements occur:
- predefined high and low watermarks (HW and LW)
- when number of free pages < LW:
	- trigger swap daemon (aka page daemon)
	- evict pages until there are HW pages available
	
## Chapter 22: Page Eviction Policy
page replacement policies:
- Want to minimise average memory access time (AMAT): $AMAT = T_M + (P_{miss} * T_D)$
- misses
	- cold/compulsory miss: when cache is empty
	- capacity miss: cache ran out of space and evicted a page
	- conflict miss: for set-associative (non fully-associative) caches when another page mapped to the same cache line evicts the page you want
	- NOTE: OS caches are always fully-associative, so conflict misses don't happen
- policies
	- FIFO / Random
		- disadvantage: can perform badly
	- LFU / LRU
		- disadvantage: overhead of updating data structures
	- Clock algorithm
		- approximates LRU
		- set up the pages in a circle, give them each a `reference` bit and sweep a clock hand over them, if the page is free, check the `reference` bit. If it is set, unset it, else, select that page for eviction
		- extension: also add a `dirty` bit, prefer evicting clean pages than dirty pages because they don't incur the overhead of writing to disk
- thrashing: when memory demanded exceeds available physical memory and the system has to constantly swap
	- admission control: kill some processes so at least some can run normally instead of trying to run everything poorly

## Chapter 23: Complete Virtual Memory Systems
Linux V Mem + x86-64:
- address space
	- user portion
	- kernel portion
		- kernel logical addresses: for kernel data structures, cannot be swapped, 1 contiguous block in physical memory
		- kernel virtual addresses: for large buffers, non-contiguous
	- programs running in user mode can't access kernel mem, need to trap to the kernel first
- page table
	- x86 provides hardware-managed, multi-level page table structure
	- 16 bit offset, 9 bits for each level (for 4 levels), 16 bits unused
- huge pages
	- (+) smaller page tables
	- (+) better TLB hit rate
	- (-) more internal fragmentation
- page eviction: 2Q replacement
	- 2 lists of pages: inactive and active
	- first access: put in inactive list
	- re-reference: move to active list
	- replacement: take from inactive list
	- periodically more from bottom of active list to inactive list, keeping active list to around 2/3 of total page cache size
- address space layout randomisation (ASLR)
	- randomise placement of different mem segments of a process (stack, heap etc.)
	- prevent attacks that use a buffer overflow to rewrite the return address in the stack, causing the process to return to some malicious instruction

# Concurrency
## Chapter 26: Intro to Concurrency
- thread context switch
	- save register state in thread control block (TCB)
	- for different threads within the same process, share address space (and hence same page table)
		- each thread creates its own stack within the same address space
- issues
	- non deterministic ordering
	- shared data
	- waiting interactions

## Chapter 27: Thread API

Thread creation:
```c
#include <pthread.h>

// creation
pthread_create(&p, // pointer to thread
	NULL, // attributes e.g. stack size, scheduling prio
	fn, // function object
	&args // pointer to the some collection of args
 );

// joining
pthread_join(p,
	&res // pointer to result variable, may need to cast
);
```

locks:
```c
#include <pthread.h>
pthread_mutex_t lock;
int rc = pthread_mutex_init(&lock,
	NULL // attributes
);
assert(rc == 0); // rc is the error code, 0 if no err

pthread_mutex_lock(&lock)
pthread_mutex_unlock(&lock)
pthread_mutex_destroy(&lock)
```

condition variables:
```c
pthread_cond_t cond;
int rc = pthread_cond_init(&cond,
	NULL // attributes
);
assert(rc == 0); // rc is the error code, 0 if no err

pthread_cond_wait(
	&cond,
	&lock,
);
pthread_cond_signal(&cond);
pthread_cond_destroy(&cond);
```

## Chapter 28: Locks
possible implementations:
- controlling interrupts
	- can be exploited by malicious programs to hog the CPU
	- doesn't provide mutual exclusion on multiprocessors
	- lost interrupts (e.g. notification that a read completed => OS can't wake the process up)
- test-and-set instruction
	- atomically updates pointer to given value and returns pointer's old value
	- spin wait with `while(TestAndSet(lock->flag, 1) == 1);`, set `flag = 0` to unlock
	- provides mutex, but doesn't guarantee fairness and has poor performance (spinning wastes CPU cycles)
- compare-and-swap instruction
	- atomic operation. Take in pointer, expected value, and new value. If pointer's value == expected, set pointer to new value and return that value, else return the old value of the pointer. 
	- spin wait with `while(CompareAndSwap(lock->flag, 0, 1) == 1);`, set `flag = 0` to unlock
	- same issues as TAS
- load-linked + store-conditional
	- `LoadLinked`: returns a pointer's value
	- `StoreConditional`: atomically takes in a pointer loaded using `LoadLinked` and a value. If the pointer was updated between the LL and the SC, return 0, else, set the pointer to the value and return 1
	- spin wait with `while(LoadLinked(lock->flag) || !StoreConditional(lock->flag, 1))`
	- same issues as TAS
- fetch-and-add
	- atomically increment a pointer's value and return the old value
	- give each caller a unique ticket number using FAA `int turn = FetchAndAdd(lock->ticket)`
	- spin wait for its turn with `while(lock->turn != turn)`
	- unlock by incrementing `lock->turn`, this doesn't have to be atomic since only 1 thread can be unlocking at 1 time
	- ensures mutual exclusion and fairness but still has performance issues

improvements:
- let threads yield the CPU instead of spinning
	- syscall that moves caller from `RUNNING` to `READY`
	- let other threads do useful work
- use queues
	- store the waiting thread IDs in a queue and wake the first one when the lock is released
	- prevents starvation

2 phase locks:
- phase 1: spin for a while, trying to acquire the lock
- phase 2: yield (Linux uses a futex)

## Chapter 29: Lock-based Concurrent Data Structures
Concurrent data structures
- Counters
	- Monitors: when calling routine, lock first, do work, then unlock when returning
	- Approximate counters: each CPU core has its own counter, global counter gets updated periodically
- linked lists
	- hand-over-hand locking: 1 lock per _node_, when traversing, acquire next lock then release current lock
- queues
	- head & tail pointers with their own mutexes, along with a dummy node for the head
- hashtable
	- concurrent LL for each bucket

## Chapter 30: Condition Variables
- _waits_ on a condition (and releases a mutex)
- _signals_ one of the waiting threads to wake up (and reacquire that mutex)
example:
```c
// signalling
void signal_cond() {
	lock(&mutex);
	done = 1;
	signal(&cond);
	unlock(&mutex)
}

// waiting
void wait_cond() {
	lock(&mutex);
	while (done == 0) { // for spurious wakeups
		wait(&cond, &mutex);
	}
	unlock(&mutex)
}
```

rules of thumb:
- hold the lock when calling `signal` or `wait`
- always use while loops to check the "underlying condition" (not the CV object itself, but the underlying variable the thread needs to check before starting up)