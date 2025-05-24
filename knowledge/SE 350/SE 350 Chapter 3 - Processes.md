Today:
- Look at representation of processes in OS
- how to control processes 

Last time:
- OS keeps a bunch of tables, rep. state of processes
	- Memory tables
	- I/O tables
	- File tables
		- Practically, maintained by file system
	- Process Table
		- Maintains process image
		- Program code, data, **Process Control Block** (PCB) that represents all info needed by OS to manage that process
		- Ex: `task_struct` in linux 
		- [[SE 350 Lab 1]] comes up with a PCB structure ourselves

Process Image & Virtual Memory
- If system supports virtual memory, needs to have translation i.e. MMU
- Code of all processes start at the same address
- From virtual add perspective, addresses "overlap". The re-maping maps virtual -> physical address
- Ex: can start virtual addresses at code 0. MMU translates these 0s to different points in physical memory.
- Why?
	- Simplifies compilation & linking of programs
	- Link s.t. code starts at a certain position
	- Different executables for each program, always at fixed (virtual) addresses. In physical memory it changes each time but executables can still run the instructions fine each time.

Process Control Block
- All info needed by OS for process
- Pretty large, some Kbs for monolithic OS
- Smaller on embedded system or [[SE 350 Lab 1]]

3 Categories of data that PCB stores:
1. Process Id: 
	- Mandatory
	- Distinguish process form other processes
	- Normally a unique number (16 or 32 bits, depends) for each process
		- Ex: `getpid()` is a system call
	- Create -> make new num
	- Destroy -> can now re-use number (PCB removed after destruction)
1. Processor state information
	- Mandatory
	- Tracks processor state
		- General purpose regs
		- Control & status regs
		- Etc.
3. Process Control Information
	- Pretty OS-dependent, but common ones:
	- Scheduling and state info
		- Mandatory
		- Process states: running, ready, waiting, etc.
		- Scheduling stuff: deadlines, priorities, etc... 
	- Data structuring
		- Linked lists (thru pointers) for child processes
		- Linked list for scheduling
		- Etc. 
	- Inter-process communication
		- Flags / signals / msgs / semaphores / etc...
	- Privileges for memory, resources, etc..
	- Memory management
		- Mandatory
		- Exactly which memory proccess owns & where
	- Resource ownership & utilization (not really needed) 


### Process Control

Steps for process creation:
1. Find & assign new unique pid (hopefully O(1))
2. Allocate space
	- Careful w/ stack of process
3. Create PCB
4. Setup appropriate PCB linkages
	- initialize fields
	- add process to scheduling queues
5. Expand other data structures
	- Ex: list of running times

When system boots, first process (root process) is init()
- `init()` is OS controlled

Process Termination:
- Normal completion
- Time limit exceeded
- Memory Unavailable
- Bounds violation
- Protection Error
- Arithmetic Error
- Time overrun
- I/O Fail
- Invalid instruction
- Privileged Instruction
- Operator / OS intervention
- Parent termination
- Parent request

More modes of execution?
- User mode
- Kernel mode
- I/O Driver
	- Different privileges
	- Usually obtained from vendors, i.e. more buggy, less trust
- Executables running on the browser
	- Less privilege on the user mode
	- "Sandboxing" execute at lower privilege than user mode
- Hypervisor Mode
	- More privileged than kernel mode
	- New more cooler registers that only hypervisor can access

Mode Switch
- Switching between different privileges
- Interrupts or System calls

Mode Switch: User -> OS:
- HW saves subset of processor regs
- HW sets PC to starting address of interrupt handler / service routine
- HW switches user to kernel mode
- SW saves other processor registers that might be modified by handler
	- Kernel stacks? 1 per process or just 1 entirely

Mode switch OS -> User:
- SW restores processor registers
- HW switches from kernel -> register, HW automatically restores processor registors
- Restoring PC causes execution to jump back to user process code
- Update PCB of the selected process
``
Process Switch:
- Save process registers in 
- Update memory-management data structures
- Restore processor registers based on PCB
	- Processor state of the selected process
	- When using virtual memory, this also changes virtual address space

Nested Interrupts
- Q: Can we allow nested interrupts? Depends on OS
	- Problem 1: Assume ISR A pre-empts ISR B. Running B -> Running A -> Finish A -> somehow go back to B (in kernel mode)
	- Solution 1: Hardware provide some mode to figure out what prev. mode was
	- Problem 2: What happens if ISR A preempts ISR B while scheduler is running (i.e. during process switch)
	- Solutions must protect important OS data strcutuers
		- Disable interrupts
		- Use concurrency mechanisms

### OS Execution

How does the OS operate?

Old OS (with/without virtual memory) or simple OS (without virtual memory)
- Execute kernel outside of any process
- OS is executed as a separate entity that operates in Kernel mode
- Kernel has its own virtual address space if virtual memory
- Kernel has its own stack
- Mode switches are expensive (using virtual memory)
- Single thread of execution in kernel space
	- No 2 processes can call the OS at the same time
	- Very bad on multi-processor

Execution within user processes:
- Most modern OS w/ virtual memory
- OS is primarily a collection of routines
- OS software executes within context of a user process
	- User process interrupted by user process
	- Routines ran in Kernel mode, but still inside the current processes' stack & context
- Performs a process switch
- **Every process has a user stack & kernel stack**
- Value of Stack Pointer changed on a mode switch
- All OS code & datastructures are mapped in virtual address space of each process


Questions:
- What does the "Kernel stack switch" when we switch between processes
	- does the PCB store the Kernel stack pointer (MSB) ?
	- We load the MSP -> PC before letting hardware load registers?
- In lab code (C / Assembly), how does the Kernel *access* the PCBs?
	- Kernels have their own stacks &  

Booting Process
- Firmware loads bootloader in main memory & runs it
- Bootloader loads portion of kernel from disk
- Process support (including all relevant tables) is initialized
- First system process (init) created
- Activate virtual memory & run init
- Init performs remaining intialization steps (possiblly load other portions of the kernels)
- Init listens to user requests & fork intitial interactive user process 

### Microkernels
- Kernel contains only essential OS functions
	- Low level memory management & protection
	- Process abstraction & basic dispatching (ex: forking)
	- Interprocess communication (msgs)
	- Interrupts
- All other services (scheduling, I/O, etc..) implemented by user processes called servers
- Interrupts are transformed into messages for servers handling (ex: the I/O)
	- The I/O is now executed inside scheduling processes

Process and Security:
- OS assigns resources & processes

2 main methods:
1. Authentication
	- Verify user is who they claim to be
	- Use something only user knows / have access to
	- When user attempts to log in, init process forks a password process
	- Password process checks the user name and password
	- If useful, password processes executes the shell/window system
	- Process then changes ownership to user
2. Discretionary Access Control
	- A set of policies dictating which subjects (users) are allowed to access which objects (OS Resources)
	- Discretionary- a subject has the right to grant access to another object
	- OS keeps a matrix of access permissions for each subject, object
	- Super user (root) can access any object
	- OS keeps track in the PCB of which user created the process (owner)
	- Ex: Superuser owns init, but password program "donated" to authenticated user

Mandatory Access Control:
- Same as DAC, but **only** the super-user can donate access rights.