#distributed_systems #consensus
**Leslie Lamport**
distributed consensus algorithm

### Basic Paxos

Reach consensus single value between multiple process.

Single acceptor? -> failure of acceptor -> no progress
multiple acceptors -> majority must accept to choose

> Value is chosen once accepted by a majority of acceptors

**Case 1:** only first value is accepted
Problem: for simultaneous proposal, every acceptor may have accepted some value but no majority. no value might be chosen

**Case 2:** multiple values can be accepted
Problem: values can be overwritten even if accepted by majority by a rogue proposer


> we need to strengthen our protocol. 2 phases are required for a proposer to learn that a value has not already been accepted by a majority. We will call the learning part a *Prepare* phase


**Case 3:** prepare a proposer first to see if value is already accepted
Problem: 

![conflict 1](./ds_images/conflicting_choice1.png)

we can see still the blue might be overwritten by red if the accept request comes later for server 3. this happens only because as s1 accept was older it didn't get to learn about blue accept before sending the RPCs. we must reject older proposals. how to know older?


**A solution:**

1. order proposals with unique number. how unique - append server numbers (which are unique for each server)
2. higher number gets priority over lower numbers
3. remember highest seen proposal number
4. remember value of the highest accepted proposal number.


**Problems with Basic Paxos**
- Liveness is not guaranteed (solution is to randomize restarts)

### Multi-Paxos

Literature doesn't cover the entirety of multi paxos implementation


- decides on a certain index of log entry
- commands must be applied in order on the state machine

**Problems**:
- lagging replicas



**Performance optimizations**
- propose messages can be omitted if the coordinator does not change between instances. This can be done by selecting a coordinator for long periods of time called *master*. This means there will be single write on every server
- batching multiple values from several application threads into a single Paxos instance for additional throughput


**Reconfiguration**
- Safety requirement: During config changes, there must no be a time when two different majorities choose different values for the same log entry.
- config changes is stored in log entry
- alpha parameter (limits concurrency, can't choose entry alpha + i until entry i is chosen)
- issue no-op commands to change config quickly


**Algorithmic challenges in Chubby**
- disk corruptions
	- checksum of content of files
	- GFS marker
	- instance that might have corrupted disk participates as a non-voting member in atleast once Paxos round
- Master leases
	- as long as lease with master - other servers can submit values
	- can server read operations purely locally
	- master maintains shorter timeout for lease than replicas - avoids clock drift
	- master churn can occur is master disconnects and reconnects with higher sequence number, this is prevented by boosting sequence numbers of current master periodically by running a complete round of Paxos
	- replica leases is also possible - helps in a system with more read traffic
- Epoch numbers
- Group membership - non trivial while engineering
- snapshots
	- not synced across replicas, each replica decides when it wants to snapshot
	- snapshot handle
- database transactions - MultiOp, f_op, t_op, guard (tests on database entries)

