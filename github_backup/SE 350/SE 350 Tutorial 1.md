Consider 64-bit microprocessor having 64 bit instructions, composed of 2 fields. First 4 bytes is opcode, and remainder is an immediate operand or operand address

a. What is the maximum directly addressable memory capacity?

4 bytes = 32 bits opcode
Remaining is 64 - 32 = 32 bits memory space for the operand address = 2^32 = 4 GB.

b. What ideal size of microprocessor address buses should be used?

64 bits because 32 bits for data and 32 bits for opcode ideal so that 1 cycle can be used per instruction.

c. How many bits should the instruction register contain if instruction register is to contain the opcode, and how many if the instruction register is to contain the whole instruction.


1.4. Consider a microprocessor generating a 16-bit address (PC and address registers are 16 bits wide) having a 16-bit data bus
a. What is the maximum memory address space that processor can access directly if connected to "16 bit memory"?

2^16 = 64kb

b. What is the maximum memory address space that processor can access directly if connected to "8 bit memory"?

Same as before, but each address stores 8 bits of data instead of 16 bits. "8 bit memory" means EACH MEMORY ADDRESS stores 8 bits, has nothing to do with number of locations in the memory space (is actually 2^16 bits).

c. What architectural features will allow this microprocessor to access a seperate "I/O space" ?
- Needs seperate I/O instruction
- At minimum 1 pin

d. If an input and an output instructions can specify an 8-bit I/O port number, how many 8-bit I/O ports can the microprocessor support?
- 

8-bit I/O port number: 2^8 

assume: 8bits / instruction is standard for course = 2 words / instruction = 1 bytes / instruction

1.8. DMA Module transferring characters to main memory from device transmitting at 10800 bits per second. Processor can fetch instructions at the rate of 1 million instructions per second. How much will the processor be slowed down due to the DMA activity.
- DMA speed = 10,800 bits per second -> 10,800 / 8 bits per instruction = 1350 instructions per second = 0.7ms per instruction
- Processor: 1 million instructions per second = 0.001ms per instruction

1.9. A computer consists of CPU & I/O device D connected to main memory M via a shared bus with a data bus of one word. CPU can execute a maximum of 10^6 instructions per second. Avg instruction requires 5 cycles, 3 of which uses memory bus. 
A memory read/write uses 1 processor cycle. Suppose that CPU continuously executing "background programs" that require 95% of its instruction execution rate but not any I/O instructions. Assume 1 processor cycle = 1 bus cycle. Suppose that very lage blocks of data are to be transferred between M and D. 

a. If programmed I/O is used and each one-word I/O transfer the CPU to execute 2 instructions, estimate the maximum I/O transfer rate, in words per second, possible through D.

Only 5% of the CPU can be used -> 10^6 * 5% = 50,000 ips = 25,000 words per second (since 1 word / 2 instruction) 

b. Estimate the same rate of DMA transfer is used

10^6 instructions per second * (5% (from read/write 1 cycle) + 0.95 * 2)

Cache
Avg access time = $H \times T_1 + (1 - H) \times (T_1 + T_2)$ = $T_1 + (1-H) \times T_2$ = $T_1 + H_2 \times T_2$

Generalizing: 
$T_s =$ sum of $T_iH_i$ from i to N
$B_i$ = Time to transfer a block from memory level $i + 1$ to memory level $i$
$T_2$ = $B_1$ + $T_1$, 

Cost = $\frac{C_1S_1 + C_2S_2}{S_1 + S_2}$ where:
- C1 average cost per bit for upper level memory
- C2 avg cost per bit for lower level memory
- S1 size of M1
- S2 size of M2

Social Intelligence Robotics Lab
 Keith Rebello
 