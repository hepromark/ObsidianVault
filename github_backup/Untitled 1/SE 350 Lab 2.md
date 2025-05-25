### Objective
1. Implement dynamic memory management system based on First Fit algorithm w/ a freelist structure
2. Implement memory-initialization function to setup RTX & microcontroller memory
3. Implement allocation & deallocation function (memory ownership)
4. Implement utility function to analyse efficiency of allocation algorithm 

#### Alignment
- We need to allocate memory in *4-byte alignments* (i.e. the smallest unit of assignment is 4 bytes + so any allocation falls on 4-byte boundaries)
- **Cortex M CAN'T store 32-bit integers on addresses that are NOT 4-byte aligned**

#### Metadata
- Each memory block (allocated or free) has metadata associated with it
```
typedef struct mem_block {
    struct mem_block* next;  // Pointer to the next block (for the free list)
    U32 size;                // Size of the memory block (excluding metadata)
    U8 allocated;            // 1 if allocated, 0 if free
    task_t owner_tid;        // Task ID that owns this block
} mem_block_t;
```
- Application should receive pointer to *just after* where the metadata is stored (ex: metadata starts 0x100 & is 8 bytes long, so user receives 0x108)

## Assignment Requirements

#### Efficiency
- O(n)
- Ex: can't just search through the entire heap everytime we need to allocate memory

#### Required Header File
- Must write a header file named `k_mem.h` that contains function prototypes & definitions

### Function Specs

**Memory Initialization** - `int k_mem_init(void)`
Desc:
- Initializes the memory manager
- Locates the heap
- Writes initial metdata to start searching for free regions
- The datastructures initialized here will keep track of free / allocated regions

Returns:
- RTX_OK (success)
- RTX_ERR (failure) if before kernel init or called twice

**Memory Allocation** - `void* k_mem_alloc(size_t size)`
Desc:
- Allocates `size` bytes according to First Fit *to currently running task / kernel* 
- Gives allocation via a pointer to that address / NULL if allocation failed
- Doesn't initialize allocated memory
- `Size` can be value
- Mutates the metadata structures (ex: update `task ID` for that region)

Returns:
- Pointer to allocated memory
- NULL if request failed (ex: not init, no mem available)

**Memory Deallocation** - `int k_mem_alloc(void *ptr)`
Desc:
- Frees memory pointed to by `ptr`
- Needs to verify if `ptr` block is valid 
	- Allocated to this task
	- Aligned correctly
- If `ptr` == `NULL`: do nothing & returns RTX_OK
- Merges free regions if this deallocation creates 2 consecutive free regions

**Utility Function** - `int k_mem_count_extfrag(size_t size)`
Desc:
- counts # of free regions less than `size` bytes
- memory management DS for blocks unused = free
- memory management DS for blocks used = allocated
