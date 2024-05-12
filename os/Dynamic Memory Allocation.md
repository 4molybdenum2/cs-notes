

userspace: malloc/ free
- malloc gets pages from OS via mmap() and subdivides them for the application

kernel space: kmalloc() / free()
- gets pages from physical  page manager and subdivides them to the memory requests in the kernel
- kmalloc - pinned pages - no page fault is seen by kernel

free() doesn't take size - memory object has metadata for size

**heap space:**
brk() -> increase virtual address space to reserve heap space
mmap() -> allocate from certain address to allocate 10 physical pages
can be many mmap regions

brk and mmap are sys calls

page frame manager - kernel level physical page manager

Simple Algorithm: Bump allocator
1. linear allocation of memory
2. simply bumps the free pointer
3. free() doesn't work its a no-op
4. typically reset free to the start of the memory buffer



**Fragmentation:**
1. Internal Fragmentation:
	1. wasted space when round up
2. External fragmentation:
	1. wasted space due to variable sized allocations
3. bump allocator doesn't have fragmentation issue



**Split and Coalesce:**
1. split: to prevent internal fragmentation
2. coalesce to reduce external fragmentation


Keep metadata with object:
1. keep header prepend metadata to object
2. on malloc(sz), look up free object of size atleast sz + size of header


**Free List**:

A **free list** (or **freelist**) is a data structure used in a scheme for [dynamic memory allocation](https://en.wikipedia.org/wiki/Dynamic_memory_allocation "Dynamic memory allocation"). It operates by connecting unallocated regions of memory together in a [linked list](https://en.wikipedia.org/wiki/Linked_list "Linked list"), using the first word of each unallocated region as a pointer to the next. It is most suitable for allocating from a [memory pool](https://en.wikipedia.org/wiki/Memory_pool "Memory pool"), where all objects have the same size.

Free lists make the allocation and deallocation operations very simple. To free a region, one would just link it to the free list. To allocate a region, one would simply remove a single region from the end of the free list and use it. If the regions are variable-sized, one may have to search for a region of large enough size, which can be expensive.

Free lists have the disadvantage, inherited from linked lists, of poor [locality of reference](https://en.wikipedia.org/wiki/Locality_of_reference "Locality of reference") and so poor [data cache](https://en.wikipedia.org/wiki/Data_cache "Data cache") utilization, and they do not automatically consolidate adjacent regions to fulfill allocation requests for large regions, unlike the [buddy allocation system](https://en.wikipedia.org/wiki/Buddy_memory_allocation "Buddy memory allocation"). Nevertheless, they are still useful in a variety of simple applications where a full-blown memory allocator is unnecessary or requires too much overhead.

**Buddy Allocation**:

In this system, memory is allocated into several pools of memory instead of just one, where each pool represents blocks of memory of a certain [power of two](https://en.wikipedia.org/wiki/Power_of_two "Power of two") in size, or blocks of some other convenient size progression. All blocks of a particular size are kept in a sorted [linked list](https://en.wikipedia.org/wiki/Linked_list "Linked list") or [tree](https://en.wikipedia.org/wiki/Tree_data_structure "Tree data structure") and all new blocks that are formed during allocation are added to their respective memory pools for later use. If a smaller size is requested than is available, the smallest available size is selected and split. One of the resulting parts is selected, and the process repeats until the request is complete. When a block is allocated, the allocator will start with the smallest sufficiently large block to avoid needlessly breaking blocks. When a block is freed, it is compared to its buddy. If they are both free, they are combined and placed in the correspondingly larger-sized buddy-block list.



Tracking free regions:
1. maintain free list
2. malloc traverses free list and find a big enough object - split if necessary
3. return ptr

Tracking free regions:
1. at the cost of some internal fragmentation
2. coalescing becomes easier by using a buddy allocator


**Segregated Pools:**
1. maintain list of specified sizes
2. round up sz and allocate


**Per thread heaps:**
1. to prevent contention for malloc/free in shared memory systems
+ global heap when per thread heap runs out




Performance Issues:

1. Single processor issue
	1. cache miss due to loss of temporal locality
	2. memory object kicked out of cache might be needed again
	3. make free list LIFO - so searching becomes faster last freed first allocated, last object more likely to be already in cache

2. Multi processor issue
	1. warm up on cpu1 can be lost if freed on cpu2 and sent to its free list
	2. should keep cpu affinity
	3. per thread  heaps can mitigate the problem, but cannot remove the problem of thread migration



**Hoard:**

1. variation of "segregated pool"
2. their pool -> Superblock, all object in a superblock are of the same size
3. each super block as a LIFO free list
4. allocate a heap for each processor/CPU not thread. and a global heap
5. try per-CPU heap first, then try global heap, if fails then get another superblock for per-CPU heap
6. when free called memory allocator doesn't return immediately return the object to the system, mem allocator keeps its own free list.
7. two threads running on two different CPUs dont need to sync each other, can maintain free list in lock free manner. disable preemption for a short amount of time, to disallow multiple threads per cpu.
8. syscall to allocate superblock
9. how to tell an object is from which superblock - suppose superblock is of size 8k (2^13), then mask out the lowest 13 bits to find the address from which the superblock start from.
	1. hoard superblock is always mapped to an address evenly divisble by its size (8k in this case)
	2. Hoard thus, doesn't need to keep per object metadata header
	3. check paper
10. Big objects in Hoard
	1. if object is bigger than half the size of the superblock just mmap() it
	2. what about fragmentation? - big objects are lesser in number as is



**False Sharing:**

True sharing: have to pay the cost for cache invalidation

False sharing: CPU 1 accessing A, CPU 2 accessing B -> but on the same cache line (object sizes are smaller than cache line size), cache has no idea what object is accessed, as long as our cache line is aliased with another cache line we are invalidating it but we don't care if we are accessing only a subset of it, this causes the false sharing issue. CPU 1 and CPU 2 keep invalidating each other.

**Solution**: dont put different object in same cache line - make object size same as cache line - 1 possible solution - lot of internal fragmentation

Active False sharing: memory allocator gives allocation from same cache lines to different processors.

Passive False sharing: when thread migrate from processor 1 to processor 2 and free it, it returns the free object to CPU 2's superblock



False sharing issues:
1. leads to performance issues
2. super linear slowdown



Hoard Paper important contribution:
1. avoiding per object metadata
2. avoiding false sharing as much as possible
3. false sharing can still happen when object is get from the global heap




**Kernel Level Allocators:**

- low level buddy allocator
- low level mechanisms allocate memory at the page granularity

kmalloc - physically contiguos memory allocation
kfree() for kmalloc

vmalloc() -> virtually contiguos chunk of memory allocation
- cannot be used for I/O buffers as they require physically contiguos memory
- allocate a large virtual contiguos memory chunk
- may sleep so cannot be called from interrupt context - use kmalloc
vfree() for vmalloc


kmalloc pages are pinned - no page faults
- for kernel performance benefits

Kernel stack size -> 2 pages (do not allocate any big stack object)
Kernel has to use heap space more actively
use kmalloc for big object allocation




**Slab Allocator**:
1. free list management scheme of the kmalloc
2. Free list:
	1. block of preallocated memory for given type of data strutcure
	2. allocate - take object from list
	3. deallocate - add object back to list
3. for individual cache - multiple slabs with preallocate physically contiguos pages are used
4. we can allocate memory from this slabs

