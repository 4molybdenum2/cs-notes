
Wide area transactions

R/W transactions: 2PC + 2PL (Paxos groups)
R/O transactions: Snapshot isolation + synchronized clocks (not perfectly synchronized)


R/W txs:
- shards in multiple datacenters for paxos groups
- locks on leader only
- choose one of the paxos to act as the tx coordinator
- if one tx can't acquire lock it has to wait -> spanner uses general 2PL for R/W txs
- Problems:
	- 2PC: if tx coordinator fail -> any txs managed by that coordinator is blocked until the coordinator comes back online. it comes back online with locks preserved
	- spanner solves this by replicating the tx coordinator
	- lots of cross datacenter messages
	- high latency from multi-site commits (~100ms)

R/O txs:
- doest use locks, 2PC, doesnt need tx manager/coordinator - avoids slowing down r/w txs
- almost 10x latency improvements for r/o txs
- needs to be serializable - results that bunch of txs must yield, must be the same as a serial order of such txs
- external consistency -> linearizability (txs must be real-time order, tx must not see stale data)
- reading latest value -> not serializable (doesnt work)
- **Snapshot Isolation**:
	- **assume: all the computers have synchronized clocks**
	- assume: every tx is assigned a timestamp (taken from the clocks)
	- for every r/w transaction - at time of commit store the values committed in a multi version database with their timestamps
	- r/o txs - choose timestamp as the start time of the tx - give me the latest data as of the start time of the read. therefore it can return values of older timestamps and thus makes it serializable (this is all assuming clocks are synchronized)
	- why is it ok to get stale value? -> because txs are concurrent, and linearizability says that two concurrent txs can be executed in either order
	- storage expense (discards records -> not mentioned by paper) ? lookup expense ?
	- allows reads from local replicas - but maybe our local replica is in the minority of paxos group and sees values from an earlier time? 
	- spanner deals with this with a notion of Safe time - if we ask for records from timestamp T but we only have records upto some T-n timestamp, then the replicas going to make us delay for getting a value
	- **assume clock synchronization is not possible**
- **Time synchronization**:
	- What if the R/O txs chooses a timestamp that is too large?
		- must wait for all of the logs to be fetched until that time - correct but slow
	- what is R/O timestamp chooses a timestamp too small?
		- violation of external consistency - miss recent committed writes
	- **TrueTime Scheme**
		- tx gets from time system - TTinterval - <earliest_time, latest_time>
		- application knows true time is somewhere between the pair of time above
		- how TTinterval guarantees to see writes done by txs before us
			- Start Rule:
				TS = TT.now().latest_time (time guaranteed to not have happened yet)
				r/o tx assigned a timestamp as it starts - start time of tx
				r/w tx assigned a timestamp as it commits - commit time of tx
			- Commit Wait (only for r/w txs):
				delay after commit and writing values and releasing locks
				Delay TS < TT.now().earliest_time
				-- guarantees - > timestamp of commit tx is absolutely guaranteed to be in the past
				
