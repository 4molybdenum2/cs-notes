
TLB - a memory cache - stores recent translations of virtual to physical memory mappings
- TLB coverage is defined as amount of memory accessible through cached mappings without incurring TLB misses.
- TLB miss -> performance penalty
- Increase page sizes?
- But increased page sizes -> higher memory footprint and higher paging traffic
- Use multiple page sizes? -> physical memory fragmentation, decreases future opportunities of using large superpages
- contiguity aware page replacement algorithm - to control fragmentation




- Having larger pages - doesn't necessarily mean we have to increase TLB size - it increases TLB coverage, because we don't have to swap in multiple pages

- Protection bit for the entire page - one reference bit, one dirty bit
- we have to write back entire page even if a small region is modified - downside - increases disk I/O



Superpage boundaries:

Issue 1: allocation
1. pages always need to be aligned
2. to allocate superpage they must start from the aligned address

Issue 2: promotion
1. create superpages out of smaller base pages
2. when to promote? can we proactively do that?
3. if too aggressive - then we are wasting physical page resources - can cause fragmentation issue
4. if too less - may lose opportunity to increase TLB coverage

Issue 3: demotion
1. convert superpage into smaller pages
2. when page attributes become non uniform (permissions...)
3. during partial page-outs
4. guaranteee correctness - paper talks about speculative demotion

Issue 4: fragmentation
1. wired pages - can't be swaped out - pinned - breaks contiguity



Prev approaches:
1. reservation
2. relocation
3. eager superpage creation - non transparent (size specified by user)
4. hardware support - contiguos virtual superpage mapped to discontiguos physical superpage
5. demotion issues not addressed - large pages partially dirty/referenced



Example:
1. if we touch 1 page of an object in memory, we will like touch all its pages - (ex: large array). This is spatial locality

Preemptible reservations:
1. opportunistic policy: go for the biggest size that is no larger than the memory object
2. if required size not available, try preemption before resiging to smaller size
3. if under memory pressure, preempt the reservation
4. best candidate for reservation - largest unused (and aligned) chunk
5. linked list is maintained to check the least recently used pages for various sizes available for reservation
6. swap out to disk is done at base page level granularity


Reference bit: denotes whether a page has been accessed in the previous clock cycle

Demotion:
1. demotions are incremental
2. performed recursively for smaller superpage sizes
3. demotion is necessary when protection attributes are changed for a part of the page
4. the system might demote active superpages from time to time to determine whether they are active in their entirety
5. On memory pressure demote superpages when resetting ref bit
6. repromote as pages are referenced
7. demote also when page daemon selects a base page as a victim page



Fragmentation:
1. restore contiguity:
	1. move clean inactive pages to a free list
2. minimize impact:
	1. prefer victim pages that contribute the most to contiguity


cluster wired pages:
1. to a separate region in physical memory dedicated to wired pages
2. so that they dont affect superpage allocations


