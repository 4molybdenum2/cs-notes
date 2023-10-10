#distributed_systems #consensus 
**Authors:** Miguel Castro and Barbara Liskov

Algorithm for state machine replication that tolerates Byzantine Faults

**Why needed**?
- malicious actors in the cluster
- actors can send wrong values
- can forge messages (solvable with public-key cryptography)

**Assumptions**:
- each node should run different OS, have diff password and admin to make it difficult for the attacker.
- leaders must be non-malicious to ensure liveness
- we assume adversary cannot  delay correct nodes indefinitely
- we assume that the adversary is computationally bound , so that it can't subvert the cryptographic hash function (ex: can't produce hash signature of a non-faulty node)

**Algorithm**:
- the replicas move through a succession of configs called *views*
- one replica is primary others are backup in a view
- views are numbered consecutively 

> In 3f+1 servers PBFT can handle f failures/malicious nodes


> B-NFS only 3% slower than NFS