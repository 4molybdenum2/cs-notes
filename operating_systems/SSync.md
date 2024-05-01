
1. Semaphore -> can guarantee happens before relation
2. Blocking lock -> puts thread on wait queue -> wakes with a signal
	1. saves cpu cycles
	2. separation of concerns


ABA Problem:
- check wikipedia

LL, SC:
1. LL sends invalidation message -> perform SC only if its first store after LL 
2. success return 1, or 0
3. Thread specific invalidation

