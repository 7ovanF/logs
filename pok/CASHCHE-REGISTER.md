> **Memory Hierarchy**
> CPU (Register) -> CASCSHE -> SRAM -> DRAM -> Magnetic Disk
> 				<- faster        slower ->

The idea: keep frequent data in faster memory, and keep less frequent data in slower memory

> [!NOTE] Principles of Memory Hierarchy
> 1. **Temporal Locality** 
> 	Jovan is the one usually making trouble, he probably is plotting something else. (if it's referenced, it tends to be referenced again soon)
> 2. **Spatial Locality**
> 	Jovan is the one making trouble, his friend Vincent must be naughty too (if it's referenced, nearby items tend to be referenced soon)

### Cache Introduction
One of the exploit of memory hierarchy is the cache, a small but fast SRAM near the CPU, often in the same chip as the CPU
- When you turn on the computer, the cache memory is empty. Then, the data from the main memory is transferred to the cache. 
- When a data is not found in the cache (later called as a cache miss), then the data will be searched from the main memory. 
- When the cache is full, then the least used data from the cache is returned to the memory. 
### Cache Block/Line
Unit of transfer between memory and cache. It's size is a multiple of words.
> Usually, a cache block is larger than a word because the overhead cost and overall transfer speed from the memory to the cache is slower than the transfer from the cache to the register (CPU), which has a transfer width size of a word.

|------------------------------------------------------------------------------| -> Block
|-------------------------------------||-------------------------------------| -> Word
|--------|--------|--------|--------||--------|--------|--------|--------| -> Byte 

$2^n$-byte blocks are aligned at $2^n$-byte boundaries

00000
00001
00010
00011
00100
00101
00110
00111
01000
01001
01010
01011
01100
### Memory Access Time
-  The average access time is the average, accounting the cache success and fail.
	$\text{Average Access Time} = \text{Cache success} + \text{Cache Fail}$
- When its a success, the time needed is the hit time, while the rate of the success is the hit rate. 
	$\text{Cache Success} = \text{Hit Rate} \times \text{Hit Time}$
- Conversely, when its a fail, then the time needed is the penalty, while the rate of the fail is the inverse of the success rate
	$\text{Cache Fail} = \text{Fail Rate} \times \text{Miss Penalty} \quad \text{Fail Rate} = 1 - \text{Hit Rate}$

> [!IMPORTANT] **Average Access Time**
> $\displaystyle{\text{Average Access Time} = \text{Hit Rate} \times \text{Hit Time} + (1- \text{Hit Rate}) \times \text{Miss Penalty}}$
> (hit rate is often written as $h$)

In a cache miss, the computer will access the cache (Hit time) before checking the main memory:  $\text{Miss Penalty} = \text{Hit Time} + \text{Memory Speed}$

$\text{Leow Vincent Vintizel}$
