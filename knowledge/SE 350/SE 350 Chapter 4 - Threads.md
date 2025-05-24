# Threads and Processes

Concepts about processes so far:
1. Resource ownership
	- OS assigns resources (most important is virtual memory address space)
	- Assigns other resources (files, I/O, etc.)
2. Scheduling execution
	- OS puts processes into a RUN state
	- Scheduling specifies the sequential execution of these processes for the core to do

We can seperate (1) & (2) into 2 different OS concepts
- Ownership stays with process
- *Threads* are for scheduling
- Each process -> multiple threads

Processes vs. Threads:
- Process:
	- pid
	- Virtual address space
	- Code & data for process (but not the stack!) because stack depends on the sequence of execution
	- Access permissions for for files, I/O resources, etc.
- Thread:
	- Thread id
	- thread state (running, ready, etc.) and scheduling info
	- processor state information
	- user stack & kernel stack

Benefits of threads:
- Fundamentally more efficient than multiple processes
	- skip resource allocation
	- ~10 times faster (new TCB & stack only)
- Less time to terminate thread than a process 
	- OS Doesn't need to release resources
- Less time to switch between 2 threads within same process
	- No need to change virtual address space
- More efficient communication between multiple execution entities
	- Threads within the same process share memory 

So far: we have **Kernel-Level Threads**
- Most common view nowadays
- Kernel schedules threads & maintains PCB / TCB

Alternative- **User-Level Threads**
- Thread management done by user-level library
- Kernel is not aware of threads- it schedule processes only
- Thread state kept by library, does not correspond to underlying process state

ULT vs. KLT:
- ULT:
	- Less switching overhead
	- Scheduling is app-specific
	- Can run on any OS
	- Don't pay interrupt overhead upon switching threads
- KLT:
	- OS calls are blocking only the thread
	- Can schedule threads simultaneously on multiple processors

Some OS provdes both solutions:
- Kernel provides thread support
- Application creates ULT
- ULT mapped on to KLT: multiple ULT can be mapped to 1 KLT

Other Arrangements are possible
- some OS allows threads to migrate between processes
- For distributed computing- thread can migrate between different nodes
- Ex: cloud computing
# Threads Implementation

# Header 3

Processes and threads in POSIX
- `fork()` call creates a new process, as a copy of current process
- `exec` fam of system calls changes current process image <-> image loaded from an executable file
- POSIX thread library (Pthread) manages threads

```
// Example Code: This is both parent and child code
...
int ret = fork()
if (ret) {
	// We are parent, do nothing
}
else {
	// We are child
	exec(new_image);
}
```

- Linux has only concept of task- either a process or a thread
- `clone()`: creates a new task by cloning a task_struct
	- Process: creates new virtual address space & copy process image
	- Thread: only creates new stacks. Code / data / PCB info shared w/ parent task
- Both fork() and pthread_create() implemented using clone()

Linux Kernel Threads
- kthreads: linux tasks that only execute in kernel mode
	- (not the same as KLT)
	- Ex: long device drivers
	- can't make system calls while in the kernel
- kthread
	- defined by task_struct
	- scheduled as normal task
	- uses only kernel memory space - no code or data in user space
	- created w/ kthread_create() kernel function

