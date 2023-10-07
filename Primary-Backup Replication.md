**Authors** Daniel J. Scales, Mike Nelson, Ganesh Venkitachalam (VMware)

Replicating the execution of a primary virtual machine via a backup virtual machine on another server.

> Model servers as *deterministic* state machines

Most servers and services have changes/operations that are not deterministic, they must be kept in sync with some extra co-ordination. Hypervisor can catch all the non-deterministic operations on the primary VM and relay to backup VM.

**deterministic replay**

VMware vSphere fault tolerance is built upon deterministic replay but adds in necessary extra protocols and functionality to build a complete fault-tolerant system. Nearly every access to shared memory can be a non-deterministic operation, therefore this scheme is not yet supported for multi-processor VMs.


### Basic Design

- backup vm on different physical server
- small time lag (two vms are in virtual lock step)
- virtual disks on shared storage
- all network inputs comes to primary
- logging channel
- output of backup dropped by hypervisor
- heartbeats + monitoring logging to ensure liveness

**Deterministic Replay**
- inputs to virtual machines - incoming network packets, disk reads, I/O
- non deterministic events - virtual interrupts, reading clock cycle counter of processor
- for non deterministic events and interrupts - exact instruction at which event occurred is recorded. during replay returned at same point in instr stream
- no need of epochs

**FT protocol**

1. Output Requirement: if the backup VM ever takes over then it is guaranteed it will continue executing in the same way that is consistent with all the outputs that primary VM has sent to the external world.
2. Output Rule: The primary VM cannot send an output to the external world, until the backup has acknowledged the log  entry associated with the operation producing the output

**Detecting and Responding to failure**
- vmware ft advertises the MAC address of a new primary on the network 
- to prevent split brain in case of network failure we can use the shared storage of primary and backup, by performing an atomic test-and-set operation on the shared storage, if it succeeds, then the vm can go live, if fails it halts
- restores redundancy in case of failure by starting new backup VM.


**Managing the logging channel**

- primary -> flush to log buffer -> logging channel -> backup log buffer -> read by backup
- slow down primary for backup to catch up
- slow feedback loop for backup to match primary speed of exec

**Operation on FT VMs**
- if primary vm is explicitly turned off, backup should not go live
- vmotion of primary -> backup disconnects and reconnects to new location
- for backup vmotion, it must request primary via logging chan to quiesce all its disk IOs temporarily.

**Implementation issues for Disk IOs**
- disk IO - non blocking - cause of non determinism, implementation of disk IO uses DMA -> non deterministic as it access the same memory pages. 
	- solution is to detect IO races and execute sequentially
- *bounce buffers* 
- reissue pending IOs during the go live process of backup VM

**Implementation issues for Network IOs**
.....read from paper honestly




### Design Alternatives
- shared disk is considered external - so writes must be delayed acc to output rule
- disadvantage
	- two non shared disk must be explicity synced when FT is enabled
	- must be explicitly re-synced after failure
	- no shared disk to deal with split brain (external tie breaker required - oracle)
- disk reads are considered inputs therefore backup never reads the disk
	- alternate is to allow backup to disk read, eliminate logging of disk read data
		- can slow down backup
		- if disk read by backup fails -> retry until succeed
		- if disk read by primary fails -> backup must be notified
		- lastly if concurrent disk read and writes on primary, write must be delayed until backup has completed read in shared disk config
		- is viable when bandwidth of logging channel is limited


Some data:
1. less than 10% performance degradation
2. 20mbps bandwidth can handle this