### Pre-Lab

Exercise 3: Manipulate CPU registers and run a thread

Exercise steps:
1. Get a new stack pointer to use for PSP
2. Setup a new thread's stack and context
3. Initiate sys call:
	4. Load registers with data from threadstack
	5. Switch current SP from MSP -> PSP
	6. Exist sys call (which will load remaining registers & jump into thread)


#### Getting a new stack pointer for PSP:
- From exercise 1:
```
uint32_t* MSP_INIT_VAL = *(uint32_t**)0x0;
```
- De-ref the 0x0 pointer inside the memory vector table, which gives us the start of memory == MSP's original location

#### Setup thread's stack and context:
- "context": stores the register values. This is loaded in/out of registers when starting / interrupting execution

1. Push 'initial' register values onto the thread's stack
2. System calls pop these values onto appropriate registers (order matters)
3. Important: PC (receiving data from the context pop) should point to instructions for that thread

**Push Order**:
xPSR, PC, LR, R12, R3, R2, R1, R0,         |            R11->R4

Example stack pointer & context pushing:
- We store some stuff inside the stack, in the order that we want it pushed
- Decrement pointer (stack grows down) each time.
```C
*(--stackptr) = 1<<24;  // xPSR code for thumb mode
*(--stackptr) = (uint32_t)my_thread_function; // PC points to function

*(--stackptr) = 0xA; // debug constant. 
*(--stackptr) = 0xA; // debug constant. 
// 12 more times ... 
```
#### Write The Sys Call

A (assembly) function invoked by `SVC_Handler_Main` (so that it can be ran in privileged mode)
- Pop 8 values from the thread's stack into R4 - R11 (hint: LDMIA)
- Update PSP register w/ current location of top of thread stack
- Return from syscall in some way that SP -> PSP instead of MSP
	- This also automatically pops 8 more values from thread stack
	- hint: `BX LR` & `magic numbers that can be loaded into LR`

## Lab


### Lab 1 Goal:
- Initiate kernel
- Dynamically create / terminate tasks
- Run multiple concurrent tasks
- Get info about initiated tasks

#### Questions to think about:
How to store TCBs
```C
typedef unsigned int U32;  
typedef unsigned short U16;  
typedef char U8;  
typedef unsigned int task_t;

typedef struct task_control_block{
	void (*ptask)(void* args); //entry address  
	U32 psp_high; //starting address of stack (high address)  
	U32 msp_high; // Kernel stack ptr
	task_t tid; //task ID  
	U8 state; //task's state. 0 == DORMANT, 1 == READY, 2 == RUNNING  
	U16 stack_size; //stack size. Must be a multiple of 8  
	//your own fields at the end  
}TCB;
```
How does kernel know which task currently executing
- value on MSP stack that tracks `tid`

How does scheduler know which tasks available for scheduling
- A queue data-structure in main.c that stores `tid`

What data does kernel need to store & access to load/unload tasks
- Thread stack location for the new thread's PSP
- current MSP location (but thats easy since already in registers)

Create Task Control Block (TCB) data structure & setup OS metadata

Functions to implement in project:
`osKernelInit(void)`
- Initializes all kernel-level datastructures & variables as required
- Called first before any other RTX functions
- Called from inside some application(?)
	
`int osCreateTask(TCB* task)`-> **haha in C poggers**
- called by application (i.e. test code)
- takes in task TCB object, *without* the TID
	- Application needs to setup TCB fields before calling this function, we just take already valid TCB object
- Modify TCB object w/ generated TID
	- A TID is an integer between 0 and N-1, where N is the maximum number of tasks that the kernel supports (including the null task), and is decided by the MAX_TASKS macro defined in the common.h file
	- Removes TID from the global `available_tids` list
	- Update by ref TCB to new TID
	
`void osKernelStart(void)`
- Called by `main.c`, after kernel initialized to run first task
- First task starts, then subsequent tasks ran via various methods including yielding / pre-emption
- **Should only be called once & called after kernel initialized**- subsequent calls ignored
- Likely used to initialize scheduling
	
`void osYield(void)`
- Halts execution of 1 task, saves context, runs scheduler & loads context of next task to run.
- Exact expected behaviour depends on state of next task (already running or new)
	- If next task already running, resume where it left off
	- If next task new, start at entry point
- At minimum, these steps happen:
1. Currently running task context saved onto task's stack
2. Scheduler ran & next task chosen
3. Thread context loaded from next task's stack
4. CPU resumes execution of next task

Pseudocode: 
1. first sys call: 
	1. this saves 8 registers -> PSP stack
	2. Push remaining registers (manually) onto PSP stack
	3. At this point, only the scheduling data structures have un-updated state
	4. Return in **kernel mode** 
	5. Stores current MSP & PSP location -> TCB
2. Scheduling logic:
	1. Scheduler picks new task: `TBC* osScheduleNextTask()` which updates scheduling struct states
	2. We now get ready to swap back:
	3. Updates processor's MSP & PSP
3. Return in **user mode** into new task
	4. Pop remaining registers (manually) from new PSP -> processor
	5. Hardware auto-pops 8 registers from new PSP -> processor
	6. Start new execution @ PC

`int osTaskInfo(task_t TID, TCB* task_copy)`
- Retrives information from TCB of the task w/ id `TID`
- Put retrieved info into `task_copy`
- Is more of an evaluation function
- Old TCB shouldn't be changed
- Returns `RTX_OK` if task exists, `RTX_ERR` otherwise

`task_t osGetTID(void)`
- For a task to find its own TID, so called from user application
- If called by Kernel behaviour can be anything
- Returns `TID` of current task, 0 if kernel not started.

`int osTaskExit(void)`
- When called by a running task, causes task to exit & scheduler called (same as osYield)
- Calling task removed from scheduler, any resources returned to OS
	- TID freed
	- Stack space freed
	- (Presumably) TCB also deleted / overwritten.
- Task state to `DORMANT`
- Return RTX_ERR if fail


------
### Random Lab Notes

Task 1 iter 1: 536964568 -> 536964568 -> 536960456
iter 2: 536956376 -> 536956376 -> 

Task 2: 536960424

want to go: 536960456, (tcb stores: 536960424)

PC target: 134220288

PC before entering osYield(): 134220868
PSP before entering osYield(): 536960496

PSP right before _asm: 536960488 
PC right before asm: 134220288 stored at 536960480

PSP upon crashing: 536960496 w/ PC: 536871472
MSP: 536960520 (very close)


