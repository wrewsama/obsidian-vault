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

