# Intro

#### Memory Management Requirements

Relocation
- We don't know where program in memory will be once its created
- During execution, program may get suspended -> goto disk -> returned to main mem at diff location
- Problem: Compiller & linker makes code that are dependent on addresses

Protection:
- Processes should only access their own section of memory
- Impossible to check abs. address at compile time- must be checked at run time by the processor

Sharing:
- Allows several processes to access the same portion of memory
- Copies of processes can share code

Logical Org:
- Programs are written in modules
- Modules written and compiled

Physical Organization:
- Memory available for program + data may not fit in memory

# Partitioning

Some basic solutions to partition memory among processes (and possibly kernel)
- These solutions don't satisfy all requirements

Fixed Partitioning:
- Memory divided into a fixed set of partitions
- Processes loaded into partitions 1-to-1

1. Equal-size partitions

2. Un-equal size partitions

Fixed-sized problems:
- Program may not fit in a partition. Programmer must design program w/ overlays
- Internal fragmentation

Assignment:
- Process can fit in any partition for equal-sized (OS can swap out (suspend) a process to make space)
- OS can swap out process to make space
- Choices exist of unequal size
	- Smallest partition that fits process chosen

Dynamic Partitioning:
- Partitions of variable length & number
- Process allocated exactly as much memory as required
- Eventually creates "holes" in the memory -> *external fragmentation*
- To solve external fragmentation

Compaction:
- Moving memory blocks around to remove *holes* in the main memory
- Expensive, since memory needs to be moved around

Placement Algorithms: the assignment of processes to free blocks:
- Best-fit: closest block in size
- First-fit: first block from beginning of memory that is large enough
- Next-fit: first block (from last placement) that is large enough

Note: 
- Best fit tends to leave small free blocks and usually performs the worst

Buddy System:
- Fast alternative to dynamic placement
- Used to allocate kernel memory
- Entire space available is treated as a single block of size 2^U
- 

# Relocation

# Basic Paging & Segmentation