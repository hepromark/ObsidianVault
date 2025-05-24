Course: https://www.youtube.com/watch?v=gfmRrPjnEw4&t=2s

#### Course Meta Info
Architecture: ARMv7
System: ARMv7 DE1-SoC

Goal: Learn principles of assembly to adapt to any assembly language, with syntax targeted towards ARM processors.


Registers:
- Area in memory very close to CPU
- Fast storage- fast write & read
- Limited storage capacity
- Each register has 8 hex values -> 32 bits of data per register
	- 32 bit register
- r0 -> r6: usually general purpose
- r7: system calls
	- Ex: r7 stores value 1. Upon system interrupts, OS reads '1' and interprets as 'end the program'
- sp: stack pointer
	- Address of next piece of memory on the stack
- lr: link register
	- Stores the location that a function returns back to
- pc: Program counter
	- Tracks the location of the next instruction to execute
	- Allows processor to move through iteratively and find where next instruction is located.
- cpsr:
	- Store special information from the program
	- Ex: subtraction of 2 numbers and the result is negative. 
	- Sets a flag that the previous result is negative
	- Has many flags that tells information about the operation (ex: carry, overflow, etc.)
	- Flags: NZCVI
		- N = negative number

Word:
- 32 bits / 4 bytes = 1 word for 32 bit registers
- Generally the max size of data per register

Stack Memory:
- Usually stored in the RAM
- Slower access & write to compared to registers
- More data available

### First Program
```
.global _start
_start:
```
- `_start` is a label for a chunk of code
- `.global` exports the `_start` label for outer programs to execute this

Program that moves data -> r0 then moves data -> r7
```assembly
.global _start
_start:
	MOV R0, #69
	MOV R7, #1
	SWI 0  @this is an interrupt
```


### Cheatsheet

Constants: `#`
Registers: `R1`
Memory Locations:
Hex values: `#0x0A`

**Keywords**
`MOV dst, src` - moves data from `src` into `dst`.
`MVN dst, src` - move negation, moves src then negate src.
`LDR` - loads data from stack into register.
`ADD dst, src, src`
`SUB dst, src1, src2` - dst = src1 - src2
`MUL dst, src, src`

`AND dst, src, src`
`ORR dst, src, src`
`XOR dst, src, src`

Logical shifts
`LSL dst, src`
`LSR dst, src`  -> is actually a division by 2
`ROR dst, src` -> rotates the bits (ex: bits 'falling out' gets pushed it to the other end)
`ROL` doesnt exist :(), use `ROR dst, 32 - n` for `ROL dst, n` equivalent

Arithmetic with flags: add a `S` behind the arithmetic
- `SUBS`

**Addressing**
Immediate Addressing: literal value to register
`MOV R0, #5`

Direct Addressing: register to register
`MOV R1, R0`

In-direct Addressing: value at an address to register
`MOV R1, [R0]`

In-direct Addressing w/ Offset: value at an address + offset to register
`MOV R1, [R0, #4]`

Pre-increment Addressing: The register being offset, R0, is incremented before accessing.
`MOV R1, [R0]!`

Post-increment Addressing
`!MOV R1, [R0], #4`

Chaining instructions for logical shifts
`MOV R0, R1, LSL #1` -> R1 shifted left by 1 bit and then moved into R0

Comparators
`CMP arg1, arg2` 
- Does `arg1 - arg2` in the background
- If arg1 > arg2: (+) number
- If arg1 < arg2: (-) number
- If arg1 = arg2: 0
- Result of subtraction's flag is stored in cpsr

Branching
`BGT location_label` -  Branch Greater Than
- Executes that branch when greater than case (the cpsr flag is checked)

`BAL location_label` - Branch Always (always executes)
- Can skip / bypass some instructions between the `BAL` call 
- Otherwise instructions ALWAYS run sequentially

`BNE` - Branch Not Equal

`BEQ` - Branch Equal

Looping

Conditional
`<cmd><cmp condition>`: Only execute `<cmd condition>` if less than
- `ADDLT` -> Only add if its Less Than for the CMP
- `MOVGE` -> Only move if greater than
- These make code more simple instead of having to do branching for every condition

Functions
`BL` - Branch Link
- Stores the address of the instruction following the `BL` inside the link register

`BX src` - Branch Back
- Branches back to the `src`
- Ex: `BX lr` after a `BAL` to mimick 'return' behaviour inside function calls of higher level languages

Preserving & Retrieving data from stack
`POP {dst1, dst2, ...}` 
- values at 1st, 2nd, ... of the stack is moved into dst1, dst2, ...
- The stack pointer gets 
`PUSH {src1, src2, ...}`