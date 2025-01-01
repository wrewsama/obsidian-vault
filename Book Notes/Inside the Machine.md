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