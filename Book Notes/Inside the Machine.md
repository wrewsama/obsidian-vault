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

