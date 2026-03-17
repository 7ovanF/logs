## Single-cycle
A whole instruction is one clock cycle. Instructions that have idle stages will still take the same time (1 cycle) to execute.

## Multi-cycle
Each stage is its own clock cycle. Solves that problem with single-cycle.

However, this implies need for a much higher clock cycle speed to rival single-cycles. For example: instructions have 5 stages, tho some have less stages. If clock cycle of multi-cycle is 5x faster than single-cycle's, it's guaranteed to be of faster or equal speed. 

Elaboration: CPI of single-cycle is 1 (yes, ofc, because single cycle for each instruction). On the other hand, CPI of multi-cycle, if each instruction has 5 stages, can be 5 (max) or less. For example, if some instructions have <5 stages, CPI can be 4.6, for example.

## Pipelining
- throughput, not latency
- potential speedup = number of pipeline stages
- bottleneck on slowest pipeline stage
- unbalanced stage lengths reduces speedup (become bottlenecks)
- time to "fill" pipeline reduces speedup
- dependencies (TODO)
All stages must have the same length.

Pipelining introduces a "race" problem. In single-cycle circuits, data operations will all have finished by the end of the instruction, so the data is safely stored before the next instruction. However, pipelined instructions have it rough.

Example: Instruction A writes something to register 8. Naturally, this would occur in the Write-Back stage. However, if we introduce instruction B, which comes right after instruction A, and specifically operates on register 8 (reads it), will that Write-Back to instruction 8 have occured by the time instruction B reads from the register?

Instructions are supposed to be sequential. A has to be considered finished before B occurs. That is the problem.
