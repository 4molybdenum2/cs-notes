#distributed_systems #consensus 
**Authors:**  Diego Ongaro, John Ousterhout
A Linearizable Replicated Log

- election safety
- leader append only
- log matching
- leader completeness - if log entry is committed in a given term, then that entry will be present in the logs of leaders for all higher numbered terms
- state machine safety - if a server has applied  a log entry at a given index for a state machine, no other server will ever apply a different  log entry for the same index



- atmost 1 leader in a given term

**Leader election**
- server initially follower - after election timeout elapses becomes candidate
- increases current term and send request vote RPCs
- candidate wins if it receives votes from majority for same term, if term changes in between it become follower
- while waiting for votes, candidate can receive AppendEntries from another leader, then it becomes follower if the received term is greater than its current term
- if candidate neither wins or loses election, then each candidate will timeout again and start new election again
- randomized election timeouts (150-300ms) to prevent split votes


**Log replication**
- once leader is elected it can receive commands from client, append it to its log and send AppendEntries RPC
- retry if follower slows down, crashes or network packets are lost
- Log matching property:
	- if two entries in different logs have same index and term, they store the same command
	- if two entries in different logs have same index and term, then the logs are identical in all preceding entries
- leader must find the latest log in the followers log where the two logs agree, delete any entries in the follower's log from that point and send the follower all of the leader's entries from that point onwards.
	- nextIndex[] -> index of the log entry that the leader is going to send to a follower
		- initialized with the last log index of the leader
		- if fails decrement for that follower and retry AppendEntries
		- optimized if the follower returns the first index it stores for the term, leader can use this to bypass all the entries for the term of the conflicting index
- safety: server can be leader for a term if it has committed all the entries for its previous terms
	- this is satisfied if we put a restriction on the election.
	- a candidate must contact a majority of the servers to win an election, and we can be sure that atleast one of these servers contains every committed entry.
	- if the candidate's log is atleast uptodate as any other log in that majority, then it will hold all the committed entries
	- raft determines which of the logs is more up-to-date by the condition:
		- if last log term of a server is more than the other, server with the higher term has more upto date log
		- if terms of the last log are same, server with larger log is more upto date


**Figure 8**:
- the leader has to check for the condition that at least one new entry from the its term must also be stored on majority of servers.


**Timing and availability**:
- if message exchanges take longer than typical time between server crashes, candidates will not stay up long enough to be elected leader, therefore no steady leader, raft cannot proceed
	- broadcastTime << electionTimeout << MTBF (avg time between failures)



**Cluster Membership changes**
- simple approach:
	- take entire cluster offline 
	- update config files
	- restart cluster
- approach:
	- change to transitional config: joint consensus



**Log compaction**:
- problems:
	- logs get huge
	- lots of memory used
	- will take long time to :
		- read and re-evaluate after reboot

- cant forget uncommitted operations
- need to replay if crash restart
- may be needed to bring other servers up-to-dat

solution:
- create persistent snapshot
- copy entire state-machine state through specific log entry
	- service writes snapshot to persistent storage
	- service tells raft it has snapshotted through some log entry
	- raft discards log before that entry
	- a server can create a snapshot and discard log at any time




- can leader execute read operations locally? (in real life read requests are higher than write requests - read request is a no-op write request)
- why might that be a bad idea? - because at some point of time we dont know if we are still the leader, when answering new requests. we must not return stale data
- how could we make the idea work? - lease based mechanism, where a leader maintains a time bound lease - as long as it holds the lease it will be leader. give lease and calculate duration from when the lease is granted at the start of the RPC call from leader.
