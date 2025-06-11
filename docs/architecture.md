# Architecture

## Process
Each program process is stored in memory, allocated with virtual memory.

## Thread
Computational concept. A process is executed by multiple thread (might for different functions).

## Hyper Threading
One ALU is allocated with two sets of Program Counter, Registers and so on. 
Thus safes time spending on context changing.

## Cache Line
Similar to buffer page. Loading a chunk (line 64bytes) of data from memory to cache since consecutive data are likely to 
be accessed during a short time period.

## Cache Consistency
If two core (ALU) access and modify same cache line, since both registers will have
a copy of the cache line, both ALU should make a protocol to ensure data in cache
line is identical.

- Example: MESI protocol. Cache line status: Modified, Exclusive, Shared, Invalid.
Based on cache lock and relies on communication between each CPU core.

## Out of order execution
Instruction will be executed out of order to enhance efficiency, it acts like serial
for single thread but might have problems in multi threads.

Some Problems about consistency:

- `this` escape problem occurs in multi thread
- Singleton is not ensured in multi thread
    - Synchronous for method
    - Synchronous for a part of method: Double check lock (better efficiency and granularity)  
        - There is also a bug in DCL singleton if evaluating by `m == null`.

Happens before order.

TWo ways preventing out of order:

- Compiler level
- Runtime memory barrier
    - It is implemented at the JVM bytecode level `.class` through specific synchronization primitives
    - Memory  Barrier in JVM: LoadLoad, StoreStore, StoreLoad, LoadStore. They prevent out-of-order operation to a certain memory address 
    (the variable decorated by `volatile`)

**Volatile**:

- Visibility
- Prevent reordering
    - JVM level barrier on memory operation
    - CPU instruction level `lock` (hotspot) by default, or other fences in some other JVMs
        - CPU level concurrent:
- Summary: CPU level concurrent control -> System IO api -> JVM instruction -> JUC



