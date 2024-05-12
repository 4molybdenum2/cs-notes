

**Linux Synchronization**
1. Memory ops can be reordered by the compiler during compiler optimization such as load and store ops - out of order execution
2. It can also be done during CPU run time: TSO (total store ordering), PSO (partial store ordering)
3. barrier() tells the compiler not to reorder - however processor might reorder
4. rmb() - read memory barrier, wmb() - write memory barrier, mb() - memory barrier

Synchronization primitives:
1. protects shared data from conurrent access
2. non-sleeping / non-blocking sync primitives:
	1. atomic operations
	2. spinlock
	3. rw-lock
	4. seqlock
3. blocking sync primitives:
	1. semaphores
	2. mutexes
	3. completion variable, wait queue
		1. Semaphore -> can guarantee happens before relation
		2. Blocking lock -> puts thread on wait queue -> wakes with a signal
			1. saves cpu cycles
			2. separation of concerns

Spinlocks:
1. most common lock in kernel
2. spins and waits for acquiring lock
3. may waste processor time while waiting


RW-locks:
1. allows multiple concurrent readers
2. when entities accessing shared data can be clearly divided into readers and writers
	1. when update - no reads
	2. when read - no updates
	3. can safely allow multiple concurrent readers
	4. can improve scalability 
3. favors readers, large number of readers can starve writers
4. seq lock - maintains a run counter - if run counter is even, write not underway, else write occuring
	1. writes favoured over writer
	2. useful when a large number of readers with a few writers


Read-Copy-Update
1. writers dont block readers
2. allows multiple concurrent readers with zero overhead
3. optimized for reader performance
4. single writer at once
	1. maintain multiple version of objects and make sure that they are not freed up until all preexisting read side critical sections are completed
	2. removal and reclamation phase
	3. reclamation is deferred






Paper:
1. synchronize_rcu waits for next CPU context switch
2. executes on all CPUs
3. to make other CPUs know about a context switch, the shared state must be updated which can lead to a cache miss, this can be expensive, therefore RCU reports per CPU state roughly once per scheduling clock tick
4. latency sensitive kernel subsystems can use inter process interrupts to to reduce latency that is normally incurred by synchronize_rcu()
5. particularly useful for deregistering NMI handlers
6. reference counting experiment - Figure 5 demonstrates RCU's advantage over shared ref counters
7. Type safe memory (retains type after deallocation) can be used where existence guarantees is required - Linux slab allocators
8. Pub-sub in linux - to dynamically replace system calls during runtimes

RCU-mediated fast and slow paths:
1. Fast path for readers, slow path for writers