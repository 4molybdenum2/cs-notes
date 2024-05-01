1. fault tolerance
2. debugging issues?
3. if consistency guaranteed -> how slow can individual servers become?
4. development issues?

LegoOS:
1. Virtually indexed cache - pros and cons
2. Cache before or after TLB?

DMA:
1. invalidating cache lines is a solution
2. create unmappable physical memory region
RDMA:
1. same as dma but data transferred across NIC to remote computer

Whenever network becomes an issue for LegoOS arch -> introduce cahce...in this case ExCache
