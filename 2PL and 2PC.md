
_How to ensure serializable schedule?

strawman sol 1:
- grab lock when tx starts
- release lock when tx ends

strawman sol 2:
- grab lock on item X before r/w X
- release lock on item X after r/w X
- allows non-serializable interleaving
- write lock must be held until tx commits

read locks must also be held until tx commits



**2 Phase Locking (2PL)**
- growing phase - tx acquires locks
- shrinking phase - tx release locks
- growing phase is entire tx
- shrinking phase is during commit

deadlock possible
- grabbing locks in order is not possible
- deadlock prevention
```
                           wait-die         wound-wait
Tn is younger than Tk      Tn dies          Tn waits
Tn is older than Tk        Tn waits         Tk aborts
```


**2 Phase Commit (2PC)**

- 2PC (Two-phase commit)
    - coordinator –> server-a: prepare-T1: server-a lock a, logs a=1,
    - coordinator –> server-b: prepare-T1: server-b lock b, logs b=1
    - coordinator –> server-a: commit-T1: write a=1 to database state, unlock a
    - coordinator –> server-b: commit-T1: write b=1 to database state, unlock b
- Now if coordinator crashes after prepare-a before prepare-b, a recovery protocol should abort T1
    - (no other transactions can read a=1 since a is still locked)
- Now if coordinator crashes after commit-a before commit-b, a recovery protocol should send commit-T1 to b.
- How does the recovery protocol work? Two options:
    - Option 1:
        - Coordinator can unilaterially determine the commit status of a transaction
        - e.g. Coordinator receives prepare-ok(T1) from server-a, but times out on server-b,
            - Coordinator can abort T1 (even if server-b has successfully prepared T1).
        - Coordinator durably logs its decision (e.g. to a Paxos RSM).
        - Recovery protocol reads from coordinator’s log to decide to commit or abort.
    - Option 2:
        - Coordinator can _not_ unilaterially determine the commit status of a transaction
        - If both server-a and server-b successfully prepared T1, then T1 must commit
            - participants log must be durable against failure (e.g. log replicated via Paxos RSM)
        - Recovery protocol must read all participating server’s log to decide commit or abort.