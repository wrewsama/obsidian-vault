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
