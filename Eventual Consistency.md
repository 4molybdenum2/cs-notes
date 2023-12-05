Dynamo - Amazon

Contribution: Combination of different techniques to provide a single highly available system that is eventually consistent.


Requirements Spec:
1. Tens of thousands of servers over multiple datacenters across the world
2. Consistency over availability
3. Simple scaling requirements
4. simple key/value interface


Query model:
1. simple read/write with key/value interface
2. object storage capacity is relatively small

ACID:
1. weaker consistency guarantess (targets "C" in ACID)

Efficiency:
1. can handle 500 requests/second and return response within 300ms

No security hazards in internal authentication systems




To attain more availability - use optimistic replication techniques which leads to conflict resolution

- conflict res in reads or writes ?
-  conflict res in application or datastore (last write wins) ?


Dynamo uses a zero-hop DHT, where each node maintains enough routing information to reroute requests to appropriate node directly.


**System Interface:**
1. get()
2. put()

- md5 hash on key to generate 128bit identifier that is used to identify the node the key is on.

**Replication:**
- consistent hashing -> to distribute load across the nodes
- assign a node to multiple points on the ring - virtual node - tokens
- coordinator replicates to N-1 clockwise successor nodes
- preference list

**Data Versioning:**
1. result of each modification is a new immutable object
2. Multiple versions of an object can be in the system at a time (due to failures)
3. can be reconciled later
4. version branching (in case of failures as well as concurrent updates)
5. client must perform reconcilation in such cases
6. vector clocks to capture causality between different versions of the same object
7. multiple branches that cant be reconciled will return all leaves
8. a problem with vector clocks -> the size of vector clocks can grow rapidly if many servers coordinates writes to an object
9. clock truncation scheme: dynamo stores a timestamp with each <node, counter> pair the last time the node updated that object , when this reaches a threshold the oldest pair is removed from the list -> can lead to inefficiences in reconcilation as descendant relationships cannot be derived accurately

