Tags:
- [[Computer Architecture]]
---
## Chapter 1: Basic Computing Concepts

The File-Clerk Model of Computing: 
> A computer shuffles numbers around, reading, writing, erasing, and rewriting different numbers in different locations according to a set of inputs, a fixed set of rules for processing those inputs, and the prior history of all the inputs that the computer has seen since it was last reset, until a predefined set of criteria are met that cause the computer to halt

fundamental parts:
- arithmetic logic unit (ALU): takes in data and code and outputs some result
	- executes instructions on data
		- arithmetic
		- memory-access
			- via absolute addressing
			- via register-relative addressing: store an address inside a register, then access that address + some offset
- storage
	- registers
		- together with the ALU in the CPU
	- RAM
		- store more data than the registers can handle
		- connected to the CPU via the _memory bus_
- bus: move numbers between storage and ALU

## Chapter 2: Mechanics of Program Execution

opcode: unique binary code used to identify an instruction type

programming model: programmer's interface to the microprocessor

CPU control unit:
- program counter
	- indicates next instruction to fetch
- instruction register
	- instruction fetch _loads_ next instruction into the instruction register
- PSW (processor status word): register on DLW-1 that stores results of arithmetic operations

fetch execute loop:
- fetch next instruction (according to the program counter) into the instruction register
- decode that instruction
- execute it
	- arithmetic: using ALU and registers
	- memory access: using memory access hardware
	- branch: using control unit and program counter (note: this is on DLW-1 where conditional branches are based on the PSW)
- all of the above are completed in 1 beat of the clock

booting up:
1. microprocessor is hardwired to fetch the 1st instruction of the BIOS
	1. stored in ROM in the motherboard
2. BIOS does basic tests on RAM and peripherals
3. BIOS loads bootloader
4. bootloader loads OS
5. OS handles all the other programs

## Chapter 3: Pipelined Execution
Intuition: split into stages, when stage `i` is done, pass the work on to stage `i+1` and start working on the input from stage `i-1`

stages: fetch, decode, execute, write back

metrics:
- instruction execution time
- program execution time
- instruction completion rate: how many instructions per unit time
- instruction throughput: how many instructions per clock cycle
- instruction latency: number of clock cycles for the instruction to pass through the pipeline

pitfalls:
- clock cycle length has to be $\geq$ the slowest stage in the pipeline, faster stages spend part of each cycle idling
- pipeline stalls: instruction getting stuck at one stage for multiple cycles. This will bubble throughout the pipeline until they exit
- pipeline refills: making the pipeline full so it can have a steady, high completion rate

## Chapter 4: Superscalar Execution
superscalar computers: can do multiple scalar operations at once

new stages: fetch, decode/_**dispatch**_, execute, write back
- dispatch determines if instructions can be executed in parallel, can think of it as a proxy that sends the instructions to the appropriate ALUs 

types of ALUs
- integer execution unit (IU): handles int arithmetic and logical instructions
- floating point execution unit (FPU): handles float arithmetic and logical instructions

memory access units:
- load-store unit (LSU): load, store, address generation
- branch execution unit: decide what to do with the program counter

instruction set architecture (ISA): abstract representation of the microprocessor's functionality (e.g. ALUs, registers, etc.) aka the _programming model_ and a set of instructions to work with those parts

microarchitecture: the hardware implementation of the ISA

microcode engine: translate instruction into machine instructions that can run directly on the hardware

Reduced Instruction Set Computing (RISC): minimise number of instructions in the instruction set and the size/complexity of each instruction, let hardware directly execute instructions

challenges:
- data hazards
	- 2 instructions use the same register such that the result of 1 affects the other
	- solutions
		- forwarding: feed ALU's result of the 1st operation into the input of the ALU 
		- register renaming: in hardware, implement more registers than required by the programming model, then map the 'architectural' registers to different physical registers
- structural hazards
	- when the processor cannot accommodate certain situations (e.g. writing to 2 registers at the same time)
	- register file: contains the CPU's registers, a data bus, and several read ports and write ports
	- a single read/write port allows access to a single register at a time
- control hazards
	- evaluation
		- pipeline stalls while branch condition is getting evaluated
		- solution: branch prediction
	- instruction load latency
		- next instruction could be in memory or even hard disk, takes time to fetch
		- alleviate using instruction caching 
		
## Chapter 5: Intel Pentium and Pentium Pro

caches
- up to 3 (L1, L2, L3)
- sit between the processor's backend and main memory
- stores instructions and data in separate halves (I-cache and D-cache)

branch unit:
- branch execution unit
- branch prediction unit
	- guess which branch to take
	- static: always take the backwards-pointing branch (because of loops)
	- dynamic: use branch history table and branch target buffer to guess based on previous branch outcomes

reservation station: buffer instructions until execution requirements are met
reorder buffer: ensure that the results of instructions executed out-of-order can be reordered later on


## Chapter 6: PowerPC Processors
instruction queue:
- for detecting and dealing with branches
- scans end of the queue for branch instructions, if the branch unit has enough info to resolve the branch immediately, replace the branch instruction with the target (aka branch folding)

## Chapter 7: Pentium 4 vs Motorola: Design Philosophy
Trace cache:
- modern x86 chips convert complex x86 instructions into a simpler instruction format (micro-ops)
- the resulting string of micro-ops from an x86 instruction is called a _trace_
- traces are cached in the L1 cache (aka the trace cache)

## Chapter 8: Pentium 4 vs Motorola: Backend
vector computing
- single instruction, multiple data stream
- exploits data parallelism

## Chapter 9: 64 Bit Computing
64 bit computing:
- data stream to the processor handles 64-bit data
- not all the data has to be 64 bits

benefits:
- wider integers
- bigger address space

## Chapter 11: Caching
caches:
- L1
- L2
- made using static RAM (SRAM)
	- has more transistors per cell than DRAM, hence faster but more expensive

locality:
- spatial: if CPU needs item from mem, it is likely to need that item's neighbours next
- temporal: if an item in mem was accessed once, it's likely to be accessed again in the near future

cache organisation:
- block frames: slots in the cache
- when requesting for a byte of data, surrounding group of bytes is also accessed, known as the _cache line_
- cache lines are loaded / stored in the block frames (they are the same size)

tag RAM:
- essentially piece of metadata associated with each block frame
- stored separately in _tag RAM_, made from really fast SRAM
- RAM block to block frame mappings:
	- Fully associative: any RAM block can be stored in any available block frame
	- direct mapping: each RAM block can only be stored in one of the block frames, but many RAM blocks could be mapped to the same block frame
	- N way set associative: Each set has N block frames, each RAM block can be mapped to any block frame within a given set

write policies:
- write-through: every write updates cache and main memory
- write-back: writes only update cache, main memory is updated only when cache line is evicted