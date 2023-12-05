#distributed_systems #gfs
**Authors** Sanjay Ghemawat, Howard Gobioff, Shun-Tak Leung

Scalable distributed file-system for large distributed data-intensive applications. Provides fault tolerance while running on inexpensive commodity hardware. Provides high aggregate performance to large number of clients.

File system interface -> distributed applications

**Assumptions:**
- component failures are norm, system made of inexpensive commodity hardware
- huge files by standard (100MB files or more, multi-gb files must be supported, small files must be optimized but need not optimize them)
- large streaming reads / smaller random reads
- most of the files are mutated by appending new data over overwriting existing data. appending becomes focus of performance optimization and atomicity guarantees. system must support well defined semantics for concurrent writes. atomicity with minimal sync is essential
- codesigning applications and file system API benefits
- high sustained bandwidth over low latency

> GFS relaxed its consistency model vastly simplify the file system without imposing burden on the applications. Also introduced an atomic append operation so that multiple clients can append concurrently to a file without extra sync.

### Design overview

**Interface**
- non standard API unlike POSIX
- snapshot operation 
- record append operation (atomic)


**Arch**
- master and chunkservers
- files are stored as multiple chunks - each chunk (is essentially a Linux file) has a unique 64bit chunk handle
- chunks replicated (normal 3 times)
- master stores metadata, chunk mappings, access control, chunk lease management, garbage collection.
- master communicates with Heartbeats with chunk servers
- clients communicate with master for only metadata info, but all data bearing ops go directly to the chunk servers.
- no caching of data (clients only cache metadata)

**Single master**
- minimal interaction btw client and master (just for metadata about chunk location). can request info about multiple chunks

**Chunk size**
- 64mb chunk size
- large chunk size advantage
	- generally large read and write operations, reduces clients need to interact with master
	- many operations on a single chunk - reduces network overhead
	- less metadata

> hotspot problem for smaller executables - fixed by increasing repl factor and staggering application start times


**Metadata**
- stored in memory on master
- does not keep persistent info about chunks - polls at startup
- heartbeats
- operation log - critical, contains historical records of all metadata changes, defines order of concurrent ops. replicated on multiple remote machines, respond only after flushing to disk both locally and remotely. batches flush.
- master replays logs for recovery at start time, checkpoints if log grows over certain size limit, B-tree form storage


### Consistency Model

relaxed consistency model #consistency

- **consistent** - all see the same data 
- **defined** - after a mutation clients will see the mutation writes in its entirety , by implication consistent
- **undefined but consistent** - after a mutation clients see the same data, but doesn't reflect what any one mutation has written.
- **inconsistent** - failed mutation makes the region inconsistent hence undefined, different clients see different data



- mutations to chunks are applied in same order to all replicas
- chunk version numbers to detect any replica has become stale or missed a mutation
- outdated chunks are garbage collected at the earliest opportunity.

> applications don't generally receive corrupted data

- checkpointing
- checksums to remove duplicate record fragments and extra padding, or filter out using unique identifiers


### System Interaction

**Leases and Mutation Order**
- mutation is done at all chunk replicas
- leases to maintain consistent mutation order across replicas
- master grants lease to a *primary* replica
- lease grant order -> primary orders mutation -> apply mutations
- lease timeout - 60s -> primary can renew lease using heartbeats

**Data flow**
- data pushed linearly across a chain of chunk servers - pipelined 
- B bytes to R replicas with T throughput elapsed time without network congestion = (B/T + RL)

**Atomic Record Appends**
- gfs provides atomic record append operation
- concurrent writes to the same region are not serializable: may contain fragments from different clients.
- in traditional write - data + offset , in record append - only data (gfs chooses offset, appends atleast once atomically and returns offset to client)
- replicas may contain different data due to retries, maybe same data duplicates
- only guarantess at least once append

**Snapshots**
- copy on write like AFS
- revokes leases on chunks -> does snapshot


### Master Operation

**Namespace management and locking**
- acquires recursive lock on namespace tree
- locks acquired in total consistent order to prevent deadlock, ordered by level in namespace tree and lexicographically within the same level.

**Replica placement**
- spread machines across racks

**Creation, re-replication and rebalancing**
- place new replicas on chunks with below avg disk space utilization
- limit the number of recent creations on each chunk server
- we want to spread replicas of chunks across racks
- re-replicate chunks for live files first


**Garbage Collection**
- lazy deletion - three days interval with regular scan
- master logs deletion immediately, renames file to hidden name, can be renamed to restore
- identifies orphaned chunks, erases metadata for those chunks
- in regular scan master responds to heartbeats with identity of orphaned chunks, and chunkservers can delete all the replicas of those chunks
- Advantages -
	- simple and reliable
	- merges storage reclamation into regular background activities (therefore can be batched)
	- safety net for accidental deletion
- Delay sometimes hinders fine tuning when storage is tight
	- can be solved by marking a file to be irreversibly deleted 
	- no replication for a file tree


**Stale Replica Detection**
- when new lease is granted - chunk version no. updated for all up-to-date replicas
- master will detect stale replica by chunk version no.
- removes stale replicas in regular garbage collection
- master also informs clients which chunkserver holds lease with latest version no. so that uptodate info is accessed
- same is performed during chunk cloning operation


> master replication if master fails
> shadow masters provide read-only access to GFS


### Fault Tolerance

**Data Integrity**
- checksumming to detect corrupt data
- chunk broken in 64Kb blocks with 32bit checksum