FaRM - not focused on geographic replication
- targets different bottlenecks than spanner
- FaRM focus on CPU time - networks on same datacenter



**Snapshot Isolation**
- A tx reads a snapshot of the database image
- can commit only if there are no W-W conflicts

> Snapshot isolation doesn't guarantee serializability in case of write skew

> Write skew is when two txs both read a certain version of the database write individual updates and then commits

Therefore snapshot isolation doesn't guarantee serializability -> non serializable interleaving tx possible under snapshot isolation