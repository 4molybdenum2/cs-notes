#distributed_systems #consensus 
**Authors:** Siyuan Zhou, Shuai Mu
Strongly consistent replication in MongoDB

**Why**
- change architecture or change algorithm?
- pull-based algorithm reduces monetary costs and gives an edge on performance
- team size, debugging time, understandability
- pull-based model -> replica fetches data proactively from another replica
- systems deployed across multiple datacenters
- backward compatibility

**MongoDB interface**:
- data as documents, stored in BSON format
- documents are identified using their unique ids and grouped in collections
- replica set, sharding - each shard deployed as replica set


**Design**:
- object for replication is Oplog
- Oplog needs to replicated to a majority to get committed
- server -> primary or secondary
- only primary handles write requests
- candidate state during transition
- timestamp is used to index oplog entries just like Raft log index
- timestamp and term together is called OpTime



**Replication**:
- secondary - syncing server
- upstream server - sync source
- PullEntries:
	- secondary sends PullEntries RPC to sync source
	- prevLogTimestamp is sent by secondary - sync source replies with oplog entries after and including that timestamp if it has a longer or same sized log, replies with empty response if it has shorter log
	- syncing server checks if entries merge from a PullEntries response, if it does it starts merging from the point of the last common entry (with the help of OpTime), and discards any diverged log entries
- UpdatePosition:
	- UpdatePosition RPC is forwarded in a chain to primary with the last log entry's OpTime
	- primary keeps a non-persistent map in memory that records the latest known log entry's OpTime for every replica
	- primary compares UpdatePosition with this record
	- if received one is newer it will update
	- later count is done on log positions of all replicas
	- if majority of replicas have the same term and same or greater timestamp it will update its lastCommitted to that OpTime
	- notifies secondaries of new lastCommitted by piggybacking onto other messages such as heartbeats and responses to PullEntries

**Correctness**:
- only checks optimes, doesn't check if sync source has higher term or same term
- can violate safety if we follow the rule -> entry considered committed once replicated on a majority of servers.
- therefore send term of syncing server in UpdatePosition RPC which will cause stale primaries to step down if higher term received and update its own term
- primary can also fetch data from replicas of lower terms as long as it has not generated new oplog entries with new term and appended it to its own oplog
- in mongodb to commit in term T a primary has to receive updateposition rpcs from a majority with term T 


- sync source selection through heartbeat rpcs
- compares oplog entries




**Extensions**:

- Speculative execution:
	- protocol can speculatively apply updates, and if needs be rollback those upgrades
	- WiredTiger storage engine
	- shows users various versions of data based on consistency levels
	- on disk checkpoints and garbage collection
	- oplog truncation by comparing latest committed entry with sync source when rollback is needed
- Initial sync:
	- mmap needs to be supported due to backwards compatibility
	- when new server joins it uses a sync source for the entire duration of initial sync
	- syncing node records the current applied oplog timestamp on the sync source as intial sync start point
	- fixing inconsistency: apply oplog entries locally
	- initial sync endpoint -> data becomes consistent
	- if failure -> new server chosen for sync
	- idempotency of operations
- Preserving uncommitted entries:
	- primary catchup phase - sync from older primary/sync-source

**Additional replica roles:**
- Arbiters:
	- can vote
	- can't store data
	- tiebreaker in elections
	- writes can be applied speculatively, but can't be committed
	- safe reconfig to change membership
- non-voting members:
	- do not participate in elections or count towards quorum
	- 50 replicas but only upto 7 voting members


**Election Optimization**:
- election handoff
	- on failovers secondaries wait for election timeout in order to detect primary not available
	- however on planned failovers this is not needed
	- use election handoff to allow a secondary to catch up to primary when primary is shutdown
	- primary uses this secondary to run election on best effort basis
	- shortens failover time
- member priority
	- priority for elections
	- pre-vote algorithm to stop unwanted disruptions / election dry-run
- read-only operations:
	- linearizable reads by piggybacking read linearization point with other concurrent entries
	- can do lease mechanism like raft?


