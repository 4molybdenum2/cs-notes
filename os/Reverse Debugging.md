
Reverse Debugging of Failures in Deployed Software

REPT: Reverse Execution with Processor Trace

If our programs are well ordered, then it is sufficient to record the happens before order of the operations. We do not need to monitor every operation, just the locks and unlocks and we can find out by deterministic replay.


**Intel Processor Trace** -> Full control flow recording system
- timestamp also recorded
- dumped into OS page -> file
- only gives control flow
- doesn't give data -> thats where offline binary analysis comes in


Backward analysis cannot infer values from irreversible instr
- forward analysis and backward analysis interchangeably


For multiple processors, we have trace from multiple PIN tools on each processor
- correct v/s incorrect execution
- possible execution v/s original execution

in order to reconstruct the memory ordering as much as possible, whenever the PT collect the branch instructions / flow instructions -> those are recorded as a part of Intel PT

additional timestamp info is recorded by PT

based on timestamp, we can assume a region was executed before another region at a **coarse grain**.


incorrect values:
1. if we cannot reason about value at a memory location
2. even if we reason correctly, in multi core if ordering is not correct, it can lead to a different ans than the original solution.

- paper makes one valid sequential execution (linearizable) even if two sequences overlap -> to reconstruct the partial order
- can lead to incorrect assumptions





