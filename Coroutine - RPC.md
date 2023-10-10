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



