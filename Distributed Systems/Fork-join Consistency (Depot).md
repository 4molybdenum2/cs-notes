
- untrustworthy nodes - malicious, faulty
- guarantees stronger consistency than eventual consistency in face of untrustworthy nodes
- fork based systems remain safe without trusting a server, but compromises liveness in two ways - first if server is unreachable, clients must block, second - a faulty server can permanently partition correct clients preventing them from observing each others subsequent updates


- signed updates 
- fork shows divergent histories
- Depot allows correct clients to join forks 
- update in Depot changes the value of a key the message format is of the form:
	- dVV, {key, H(value), logicalClock @ nodeId, H(history)}
- node N increments its logical clock on each local write
- when node N receives update from another node, it increases its logicalClock to exceed the logicalTimestamp of the received update
- two updates are concurrent if neither appears in each others history

Conditions for a node to accept updates:
- must be properly signed
- updates must be newer than any updates already received from the signing node
- receiving node's version must include the signing node's dVV
- history hash of receiving node must match hash computed by C across every node's last update at time dVV.
- u's timestamp must be atleast a constant times C's current wall-clock time



**Handling Conflicts**:
1. filtering
2. replacing


FCC (fork causal consistency) requires that once two nodes have been forked they can never observe each other's updates after the fork junction.

Paritioning nodes in such a way is unacceptable in many systems. depot handles this by a property called: **joining forks**


**Joining Forks:**

To join forks, nodes use a simple coping strategy: they convert concurrent updates by a single faulty node into concurrent updates by a pair of virtual nodes. A node that receives these updates handles them as it would “normal” concurrency: it applies both sets of updates to its state and, if both branches modify the same object, it returns both conflicting updates on reads (conflict resl). We now fill in some details.



