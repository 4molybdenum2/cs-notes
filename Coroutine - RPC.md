#distributed_systems 
Co-operative task management without manual stack management
(event driven programming is not the opposite of threaded programming)


**Authors**: Atul Adya, Jon Howell, Marvin Theimer, William J. Bolosky, John R. Douceur (Microsoft Research)


High performance programs are written using pre-emptive task management, where tasks can interleave in single processor or overlap in multi-processor

Another approach is serial task management where there is no concern for accessing the shared memory state without corruption of  -> strategy is inappropriate when user want to exploit multiprocessor parallelism.

Co-operative task management -> task's code only yields control to other tasks at well defined points in exec

cooperative task management -> uses events


- optimistic v/s pessimistic locking
- data partitioning to reduce number of conflicts

**RPC performance:**

threads:
- easy to understand, automatic stack management
- con: C10K problem, each thread takes up a lot of resources


event:
- pro: fast / no pre-emptive scheduling, no "thread safe" requirement (with data partitioning)
- con: harder to read/write, stack-ripping (function scope, automatic variables, control structure, debugging stack), harder to evolve -> divide the code to pass event handlers to register functions


sweetspot:
- userspace thread (fiber, stackful coroutine)
- switch between them on user call
- similar to threads but at lower cost


stackless coroutine:
- callbacks async, await
- callback hell?




**At-least once RPC behaviour:**
- waits for a while - if timeout retry
- not OK for ops with side effects
- OK for ops like reads (which can be repeated with no side effect)
- ok if application can handle duplicate requests



**At-most once RPC behaviour:**
- server code detects duplicate request
- must store unique id - how?
- when is discarding old ids safe?
	- unique client id
	- per-client RPC sequence numbers
	- - client includes _"seen all replies `<= X`"_ with every RPC much like TCP sequence \#s and ACKs
	- or only allow client one outstanding RPC at a time s.t. arrival of `seq+1` allows server to discard all `<= seq`
	- or client agrees to keep retrying for `< 5` minutes server discards after 5+ minutes
- How to handle duplicate request while original is still executing?
    - Server doesn't know reply yet; don't want to run twice
    - _Idea:_ "pending" flag per executing RPC; wait or ignore

What if an at-most-once server crashes?
- if at-most-once duplicate info in memory, server will forget
    - and accept duplicate requests
- maybe it should write the duplicate info to disk?
- maybe replica server should also replicate duplicate info?


**Exactly once?**
- two general's problem




