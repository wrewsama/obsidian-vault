## Purpose of an OS

- Abstraction of hardware
- Resource Allocation
- Control Programs (controls exec to provide security and prevent improper use)

## Kernel

- core of the OS
- purpose:
	- serves as an interface between hardware and software
	- provides syscall interface
	- handles IPC, scheduling, interrupt handling etc
- is the first part of the OS that loads into mem when computer boots up
- remains in mem until the OS is shut down

## Microkernel vs Monolith

Microkernel:
- user services are separated from kernel services in memory
- Kernel is generally more robust and extensible (more things running in user mode instead of kernel mode)
- Better isolation and protection between kernel and high level services

Monolith:
- user and kernel services share same memory space
- Better performance (no overhead of message passing, provided by OS, between user and kernel services)

## Paging and Segmentation

Paging:
- mem context is split into fixed size pages
- there is a page table that maps page no to frame no in physical mem

Segmentation:
- mem context split by segments (data, heap, etc)
- seg table contains base addr in physical mem and size limit

Segmentation with paging:
- split by seg then by page
- Each segment of each process has its own page table
- Segtable now stores page table base instead of base phys addr

## Memory
**Logical Memory** 
- provides a layer of abstraction that allow programs to access memory without dealing with the actual physical memory locations.
- The OS will:
	- allocate logical address space to the process
	- generate logical addresses while the process is running
- page table tracks the mapping of logical addresses to physical addresses
- Translation Look-Aside Buffer (TLB) caches some of the logical address translations in the page table

**Virtual Memory**
- unused pages are stored in secondary storage acting as virtual memory as a swap file
- page table also tracks if a virtual page is in mem or secondary storage
- virtual mem allows the OS to perform as if it has additional RAM

**Benefits of Logical Memory**
- can use virtual memory
- allows for techniques like paging and segmentation by mapping these non-contiguous memory addresses to contiguous logical addresses for programs to use
- allocates separate logical address space for each process, preventing them from interfering with one another
## Deadlock

4 conditions:
- mutual exclusion
	- only 1 process can use a resource at 1 time
- hold and wait
	- process holds 1 resource while waiting for another
- no preemption
	- cannot take resources away from a process
- circular wait
	- the waiting processes form a cycle

Handling deadlocks:
- Prevention: ensuring none of the 4 conditions are satisfied
- Avoidance: when process requests resource, algo examines the resource allocation state, if that allocation causes the state to be unsafe, reject the req
- Detection/Recovery: abort (either all processes at once, one by one till deadlock is  resolved, or taking resources from lower prio to higher prio processes one by one till deadlock resolved
- ignorance: just reboot

## Scheduling
Criteria:
- fairness
- utilisation

metrics:
- waiting time (arrival to 1st share of cpu)
- turnaround time (arrival to end)
- throughput (total tasks / unit time)

examples:
priority, first come first serve, round robin, MLFQ

**MLFQ**
adaptive scheduling algo

Basic Rules:
- If prio(A) > Prio(B), A runs first
- If prios are the same, they run in RR

Prio setting/changing rules
- new job -> highest prio
- Job fully utilise time quantum -> reduce prio
- Job gives up / blocks before finishing time quantum -> prio retained

## Execution Contexts
Contexts:
- Memory: Text, Data, Heap (Page Table)
- OS: Process id, process state, file descriptors, etc
- Hardware: registers, FP, SP, TLB

Saved in the Process Control Block for each process

## Processes vs Threads

| Threads                                                                  | Processes                              |
| ------------------------------------------------------------------------ | -------------------------------------- |
| Only have separate hardware contexts, OS and memory contexts are  shared | Entire execution context is duplicated |

Pros and cons of threads over processes

Benefits
- Economy
	- less resources to manage compared to multiple processes
- Resource sharing
	- No need additional mechanism to pass info around since there are shared resources
- Responsiveness
	- can appear more responsive
- Scalability
	- multithreaded programs can utilise multiple CPUs (though processes can too)

Problems
- Syscall concurrency
- possiblility of parallel syscalls and re-entrancy problems on global variables
- Process Behaviour
	- Process operations get affected, no isolation
	- e.g. if 1 thread does exit() or exec() or fork(), what happens to other threads and the process

## Linux Boot Process
1. BIOS/UEFI boots up
	1. Initialises hardware
	2. UEFI is essentially a better BIOS
		1. faster
		2. secure boot
2. BIOS/UEFI runs Power On Self Test (POST)
	1. ensures hardware is working before fully turning everything on
	2. throw error if any
3. find and load boot loader software
	1. usually GRUB2
	2. locates OS kernel on disk
	3. load kernel into mem
	4. run kernel code
	5. hands over control to kernel
4. kernel takes over computer's resources
5. loads hardware drivers and modules
6. mounts the root filesystem
7. run init process (usually systemd)
	1. initialises everything that needs to launch behind the scenes when starting up linux

## How Linux runs a program
* Client request to run application
* Shell informs kernel to run binary
* Kernel allocates memory from the pool to fit the binary image into
* Kernel loads binary into memory
* Kernel jumps to specific memory address
* Kernel starts processing the machine code located at this location
* Kernel releases memory back to pool once code has completed execution

## How Linux opens files
- kernel receives `open` syscall
- kernel looks up the directory entries along the provided path to find the inode corresponding to the file
- kernel creates an entry in the system-wide open file table
	- tracks state e.g.
	- current offset
	- access flags (e.g. NONBLOCK)
- kernel creates an entry in the process's open file table (aka the file descriptor table) and returns the index of that entry, aka the _file descriptor_
## Async IO

**O_NONBLOCK**
* Set using `fcntl` on a file descriptor
* reading: if no data is available, return immediately with err instead of blocking
* writing: if there's no space available in the write buffer, return immediately with err instead of blocking

**io_uring**
* contains 2 ring buffers
	* submission queue
	* completion queue
	- these are in a region of memory shared between user and kernel space => zero copy
- IO requests (e.g. read/write) are sent to the submission queue
	- several requests can be submitted at once => less context switches & syscalls => better performance
- once done, they'll be put into the completion queue


**poll vs select**
- poll has no fd limit
- poll only checks the fds in the `pollfd` array while select has to scan all fds up to the max value

**epoll vs poll**
- epoll is only on linux
- poll needs to scan through all it's monitored fds (O(N))
- epoll maintains the list of fds in the kernel => O(1)
- thanks to the better performance, epoll is better for large numbers of simultaneous connections (e.g. Redis server connections)

## Hard vs soft links

|                       | Hard                                                                                      | Soft                                                                                                                |
| --------------------- | ----------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------- |
| Command               | `ln original link`                                                                        | `ln -s original link`                                                                                               |
| Implementation        | Directory entry is a pointer to the original file's inode, reference count is incremented | Is a special file with its own inode that stores the path to the original. The kernel redirects to that stored path |
| Deletion of original  | Hard link will still exist, inode reference count will be decremented                     | Soft link becomes dangling                                                                                          |
| Different filesystems | cannot span different filesystems                                                         | can span different filesystems                                                                                      |
