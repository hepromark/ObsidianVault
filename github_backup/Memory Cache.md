### Overview
Programmers want fast memory and lots of it
- Almost all applications / programs access memory, and most of the runtime is spent on accessing memory.
- Fast memory is usually expensive (money and space-wise)
- Higher density memory can store more data but is slow to access

### Memory Hierarchies

Using **memory hierarchies**, we can create the 'illusion' of all memory in the computer being as fast as the fastest available memory.
- Memory hierarchies exploit the **Principle of Locality**

#### Principle of Locality
A program access a relatively small portion of their address space at any instant of time
1. Temporal locality: if an item referenced, it tends to be ==referenced again soon==
2. Spatial locality: if an item referenced, item whose ==addresses are close== tend to be referenced again soon


This hierarchy contains multiple levels of memory w/ different speeds and sizes
- Faster memory & less data is near the processor
- Slower memory & more data is further from the processor
- Data at closer layers are a *subset* of the data further away (i.e. there is duplication of the same info)

![[Pasted image 20250112150738.png]]

**Block**: The smallest unit of information that may exist / not exist at each level

If data requested by the processor appears in a block in an upper level: **hit**
If data not found: **miss**

![[Pasted image 20250112142900.png]]

**Hit rate**: fraction of memory accesses found in the upper level
**Miss rate**: $1 - \text{hit rate}$, the fraction of misses to total accesses.

Performance is main driving reason for having memory hierarchies, so the time for each hit / miss important:
- **Hit time**: Time to access upper level = time to check if hit or miss + time to access memory
- **Miss penalty**: Time to replace a block in upper level w/ corresponding block from lower level + time to deliver block to processor (?)
- Hit time is much smaller than miss penalty


### Memory Technologies
There are 4 main memory technologies:
- Main is memory (RAM) is DRAM
- Closer to processor (nowadays directly inside the CPU) is SRAM
- Flash memory for long-term, non-volatile storage

## Cache

#### Simple Cache Example
The cache contains a collection of recent references: $X_1, X_2 \rightarrow X_n-1$
- Processor requests word $X_n$ that is NOT in the cache: cache miss
- $X_n$ brought from memory into the cache

How do we know if a data item is in the cache?
- Simple method: **direct mapped** cache structure
- Each memory location mapped directly to 1 location in the cache

Direct mapped mapping:
$\text{Index} = \text{Block Address } modulo \text{ Number of blocks in the cache}$
- If number of entries in the block is a power of 2; modulo = the low-order ${log}_2$ 
- Ex: 8-block cache uses the lowest three bits of the block address

![[Pasted image 20250112152457.png]]

Problem: cache index -> memory block is one-to-many:
- Need to use **tags** inside the cache for each cache block
- Tag: field in a table that contains address info required to identify a requested word exactly in a cache
- Ex: **tag** for an 8-block cache is the remaining 2 bits (last 3 bits is already used)

Problem: Upon start-up / other cases, values in the cache may be uninitialized / invalid:
- **valid bit**: indicate if an address is valid

### Reading Caches

The general flow to accessing memory is:
1. Binary address sent to cache
2. Binary address compared to corresponding index + tag field of data stored at that index
	- Ex: 00011 -> look at index 011 for tag 00 
3. If hit: data sent back
4. If miss: (ex: 00011)
	- Go to address 00011 in lower layer
	- Copy data into Cache index 011 (overwrite existing data if needed)
	- Update tag to 00

Q: How big should the tag field be?
- Should contain enough bits to perfectly id a specific memory location inside a block
- Ex: 64-bit address, direct-mapped cache, $2^n$ blocks in the cache, $2^m$ block size:
	- n bits are used for the index 
	- m bits are used for the word inside the bock.
- Tag field size = $64 - (n + m + 2)$
	- n is covered by block id
	- m is covered by word size
- Total size of cache: $2^n \times (\text{block size} + \text{tag size} + \text{valid field size})$ bits