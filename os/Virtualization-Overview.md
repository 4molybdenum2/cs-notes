
**Overview**
- Virtualization is a method to run multiple OS and user applications on the same hardware
- Virtual Machine Monitor (VMM) or hypervisor the software layer that allows several virtual machines to run on the top of the same hardware

Type 1 hypervisor - runs directly on hardware
Type 2 hypervisor - runs on host OS


Virtualization principles:
1. Fidelity: Software on VMM executes identically to its execution on hardware
2. Performance: majority of guest OS instructions run on hardware without VMM intervention
3. Safety: VMM manages hardware resources


Solution 1: Full emulation



Solution 2: Trap-and-emulate

Types of instructions:
1. Privileged instructions -> only executed in privileged mode
2. Sensitive instructions -> modifies privileged machine state
3. Safe instructions

x86 difficulties:
1. non virtualizable processor
2. not all sensitive instructions are privileged
3. they do not trap and behave differently in kernel and user mode
4. As guest OS runs in higher rings, its these sensitive instr gets silently ignored
5. what should happen is a trap so that VMM can emulate this in ring 0

Possible solutions:
1. interpret every inst -> very slow (Virtual PC on Mac)
2. Binary translation -> rewrite non virtualizable inst (VMWare)
3. Paravirtualization -> modify guest OS to avoid non-virtualizable inst (Xen)
4. Change hardware -> Add **new CPU mode**, **extend page table**, other hardware assistance





