## Pre-emptive Multitasking
#### Objectives:
- Dynamically allocate task stacks based on user desired sizes
- Assign task priorities based on deadline
- Pre-emptively switch tasks (time slices)
- Allow running tasks to change prio (via deadlines) of others
- Allows tasks to sleep (not scheduled for a while)

**SysTick**
- CortexM chips have onboard timer called SysTick, called **SysTick_Handler**
- Integer loaded into register; decremented every clock cycle
- Once integer -> 0, integer reset & interrupt triggered
- ISR triggered inside *core/src/stm32f4xx_it.c* inside SysTick_Handler

#### Suggested Pre-lab: **Pre-emptive Round-Robin Scheduling**
1. Every task has a variable stored inside the TCB
	- Represents remaining time (ms) before task must be switched out = *time slice length*
2. Whenever a running task expires, context switch should be triggered & round-robin scheduler should run
3. Try adding new task that calls osYield (with a timeslice value) 
4. **Make sure race condition doesn't happen**: inside context switch + an interrupt occurs account for.

### Modifications
- [x] Deadline in TCB ✅ 2025-03-31
	- [x] **rem_sleep_time** in TCB ✅ 2025-03-31
- [x] SLEEPING (3) task state ✅ 2025-03-31
- [x] **osTaskInfo** contains new added deadlines ✅ 2025-03-31
- [x] Dynamic stack allocation ✅ 2025-03-31
- [x] osYield() resets task time remaining back to its deadline & timeslice ✅ 2025-04-03
- [ ] Pre-emptive EDF scheduler (earliest deadline first) - tie break w/ Task ID
- [x] osKernelStart enables timer-based task switching ✅ 2025-04-03

### New Functions
`void osSleep(int timeInMs)`
- Yields current execution 'immediately'
- Removes task from scheduling queue (state -> SLEEPING & into SLEEP queue) for `timeInMs`
- After waking: task back to READY queue + deadline reset
- Essentially osYield() + some sleep time

`void osPeriodicSleep()`
- "Finished this period's execution"
- Sleeps until end of current period: 
	- `rem_sleep_time = periodic_deadline - rem_deadline`
- Can just call `osSleep` in the background but kernel calculates sleep time

`int osSetDeadline(int deadline, task_t  TID)`
- Sets deadline for the task w/ Id TID
- **Blocking function**
- If new set deadline < deadline of current running task: pre-empt the current task

`int osCreateDeadlineTask(int deadline, TCB* task)`
- Just regular `osCreateTask()` but with deadline + dynamic stack sizes

Questions:
- What counts as still "being" in an ISR? Branching out from the pendsv doesn't count as "leaving it" right?

## test2  
If a task never yields, when its deadline is reached the kernel will pre-empt it (with the currently most urgent task). When the task is resumed later it will continue from where it was interrupted.  
	  
### expected output  
```  
---- test2 ----  
A0 =0, B0 =0  
A1 =[8000-40000], B1 =1  
A2 =[16000-80000], B2 =2  
A3 =[24000-120000], B3 =3  
A4 =[32000-160000], B4 =4  
<serial timeout>  
<end of test>  
```  
### your output  
```  
---- test2 ----  
A0 =0, B0 =1  
A1 =810731, B1 =2  
A2 =1624103, B2 =2  
A3 =2436638, B3 =2  
A4 =3249205, B4 =2  
<serial timeout>  
<end of test>  
  
```  
### your score: 0.20/1.00  
  
## test3  
Task stacks are created dynamically using k_mem_alloc, and are freed on osTaskExit. (If the heap is exhausted the task will not be created and will return RTX_ERR.)  
  
### expected output  
```  
---- test3 ----  
creating tasks with 0x2000 stack size until stack space is exhausted...  
created task with tid-1, stack_high=0x2000cc30  
created task with tid-2, stack_high=0x2000ec40  
created task with tid-3, stack_high=0x20010c50  
PASS: osCreateTask failed (expected)  
  
starting tasks...  
task-1  
task-2  
stack_high: 0x2000ec40  
exiting  
  
task-3  
creating task  
stack_high of created task: 0x2000ec40  
PASS: task stack freed & reused  
   
---- test3 ----  
creating tasks with 0x1000 stack size until stack space is exhausted...  
created task with tid-1, stack_high=0x20009554  
created task with tid-2, stack_high=0x2000a56c  
created task with tid-3, stack_high=0x2000b584  
created task with tid-4, stack_high=0x2000c59c  
created task with tid-5, stack_high=0x2000d5b4  
created task with tid-6, stack_high=0x2000e5cc  
created task with tid-7, stack_high=0x2000f5e4  
created task with tid-8, stack_high=0x200105fc  
created task with tid-9, stack_high=0x20011614  
created task with tid-10, stack_high=0x2001262c  
created task with tid-11, stack_high=0x20013644  
created task with tid-12, stack_high=0xfbc  
created task with tid-13, stack_high=0xfbc  
created task with tid-14, stack_high=0xfbc  
PASS: osCreateTask failed (expected)  
  
starting tasks...  
task-1  
task-2  
stack_high of exiting task: 0x2000a56c  
task exiting...  
task-3  
creating new task  
task-4  
stack_high of newly created task: 0x2000a520  
FAIL: task stack not freed/reused  
  
task-5  
task-6  
task-7  
task-8  
task-9  
task-10  
task-11  
task-12  
<serial timeout>  
<end of test>  
  
```  
### your score: 0.50/1.00  
  
## test10  
Test10 Timing accuracy -- two tasks both execute on time within +-0.5ms accuracy (e.g. slide 2 of clarifications post)  
  
### expected output  
```  
---- test10 ----  
Using DWT for timing  
  
measured execution time of Task B: 7209 us  
  
Timestamps of Task A (deadline 4ms, execution time ~0.01ms)  
A start1 =[0+-500] us  
A start2 =[4000+-500] us  
A start3 =[8000+-500] us  
A start4 =[12000+-500] us  
A start5 =[16000+-500] us  
A start6 =[20000+-500] us  
A start7 =[24000+-500] us  
A start8 =[28000+-500] us  
A start9 =[32000+-500] us  
A start10 =[36000+-500] us  
  
Timestamps of Task B (deadline 11ms, execution time ~7.2ms):  
B start1 =[10+-500] us, B end1 =[7220+-500] us  
B start2 =[11000+-500] us, B end2 =[18220+-500] us  
B start3 =[22000+-500] us, B end3 =[29220+-500] us  
  
<serial timeout>  
<end of test>  
```  
### your output  
```  
---- test10 ----  
Using DWT for timing  
  
measured execution time of Task B: 7207 us  
  
Timestamps of Task A (deadline 4ms, execution time ~0.01ms)  
A start1 =11 us  
A start2 =3207 us  
A start3 =7207 us  
A start4 =11206 us  
A start5 =15206 us  
A start6 =19207 us  
A start7 =23207 us  
A start8 =27206 us  
A start9 =31206 us  
A start10 =35207 us  
  
Timestamps of Task B (deadline 11ms, execution time ~7.2ms):  
B start1 =15 us, B end1 =7287 us  
B start2 =18206 us, B end2 =25472 us  
  
<serial timeout>  
<end of test>  
  
```  
### your score: 0.19/1.00  
  
## marking comments  
{notes
