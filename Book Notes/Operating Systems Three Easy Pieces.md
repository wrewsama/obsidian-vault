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