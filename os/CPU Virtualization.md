


Binary Translation:

1. replaces sensitive inst with non sensitive ones
2. on demand trap and emulate
	1. challenges:
		1. relative memory addressing
		2. sensitive instructions

Dynamic Binary Translation:
1. input is binar x86 code
2. translation happens at runtime
3. on demand: code is translated only when it is about to execute
4. system level: rules set by x86 ISA
5. subsetting: safe instr
6. adaptive: translated code is adjusted based on guest OS changes


Translation cache is required -> to not redo the same code translation again and again

- If somehow interrupt bit is emulated virtually in the CPU, then a lot of instructions can be inlined.




**Intel VT-x HW assisted CPU Virt:**

Two new modes of execution:
1. VMX root mode -> same as x86 without virt
2. VMX non-root mode -> runs VM sensitive instr cause transition to root mode, even in Ring 0

Mode switch:
1. VM entry: root -> non root
2. VM exit: non root -> root


- whenever sensitive instructions in guest OS -> trapped
- make transition from non root -> root mode
- basically whenever sensitive instruction ins trapped it can be handled by the hypervisor


Intel VT-x introduced **Virtual Machine Control Structure** (VMCS) -> for each individual virtual processor,  introduction of multiple vCPUs at hardware level
1. 1 VMCS for 1 virtual processor
2. separate VMCS for VMs
3. VMCS is exposed to the upper software stack to represent a vCPU
4. Hypervisor can configure, which sensitive instruction cause VM exit, let others be handled by guest OS
5. syscall registers virtualized as a part of VMCS data structure
6. guest OS can execute by staying in non-root mode, but changing ring from 3 to 0
	1. guest OS -> hypervisor
	2. hypervisor -> guest OS
	3. these are context switches
	4. state of guest OS / hypervisor needs to be stored and restored on context switches
	5. VMCS includes state of the hypervisor and the VMs


Binary Translation v/s VT-x
