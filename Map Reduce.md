#distributed_systems #mapreduce
**Authors**: Jeffrey Dean, Sanjay Ghemawat (Google) 2004

Map-reduce is a programming paradigm for working with large datasets.
- Two functions **map** and **reduce** are designed. The first one for converting the input dataset into key-value pairs and the second to combine all the pairs associated with the same key.
- easily parallelize on a large set of commodity machines.


**Why it was developed?**
- parallelization
- fault tolerance and load balancing
- abstraction for fast development and usability
- high performance

**Doing it the hard way**
- send-recv calls
- terabytes of data transferred over wire
- coordination between machines required for each task
- debugging was a pain
- data backup was a problem (for such large data)
- scaling was an issue


**Solution required**:
- something as a library to integrate into user code
- no persistence
- combine with GFS?
- online tasks
- automatic parallelism, load balancing and fault tolerance

**Programming model:**
- map (k1,v1) -> list(k2, v2)
- reduce(k2, list(v2)) -> list(v2)


**Examples of Map-reduce:**
- Distributed grep (key = pattern, value = string that matches the pattern)
- count url freq (key url, value = count of url)
- reverse web link graph
- term vector per host
- inverted index
- distributed sort


**Google environment:**
- dual processor x86 machines running Linux with 2-4 GB of memory
- commodity networking hardward 100 mbps or 1gbps
- cluster of hundreds or thousands of machines
- storage inexpensive IDEs, filesystem = GFS
- users submit jobs to a scheduling system, each job -> tasks -> scheduler


**Execution:**
- input processed in M splits (16Mb - 64Mb) and intermediate key space is partitioned into R splits, partitioning function and R is specified by the user,
- single master, rest workers, M map tasks, R reduce tasks.
- k-v pairs buffered in mem, periodically written to local disk
- reduce worker sorts intermediate k-v pairs -> passes to reduce function
- Results available in R output files

**Fault tolerance:**
- master pings workers periodically
- if worker fails, marks worker as failed and resets task back to idle state. completed map tasks needs to be re-executed again because the results are stored on the local disk of the machine that failed and might be unavailable. completed reduce tasks dont need to be rescheduled because results are in a global file system.
- all workers are notified of a re-execution of a failed map task.
- master writes periodic checkpoints, aborts map reduce computation, users can retry.
- atomic commits of map and reduce tasks to produce non-faulting sequential execution of a program.

**Locality of data**
- master tries to schedule map task on machine that contains replica of input data or if not available, then on a machine near the input data. most input data is read locally.
- master makes O(M+R) scheduling decisions, keeps O(M * R) states in memory.
- M is chosen such that individual task consumes 16Mb - 64Mb

**Backup tasks**
- handles stragglers.
- close to completion -> initiates backup tasks

**Refinements**
- partitioning function
- ordering guarantees
- combiner function - partial merging of data sent over the network (same code as reducer, output is written to intermediate file, speeds up certain Map reduce operations)
- skipping bad records (installs signal handler catch seg faults and bus errors, last gap UDP packet)
- local execution for debugging
- counters

**Performance**
- When the paper was written 1TB of data was huge
- 100-200Gbps of aggregate bandwidth
- roundtrip time less than 1ms
- overhead due to propagation of program to all worker machines