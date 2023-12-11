Updating Google's web index is a challenge
- main problem is performing small incremental updates
- Google's MapReduce is more suitable for creating large batches for efficiency
- we require an incremental processing system
- strong consistency required for the transactions
- Computation should be large in some aspect - total CPU or memory requirement, i.e; smaller computations not suited to MapReduce / BigTable
- Relaxed latecny
- tx subject to write skew under SI model of Percolator



observer framework -> linked to Percolator worker to look for changes in Bigtable rows




**Lightweight Lock Service:**
1. high throughput required
2. lock server needs to be replicated (to survive failure), distributed and balanced (to handle load) and write to a persistent data store
3. Bigtable satisfies our needs so locks are stored in a special in-memory columns
4. writing tokens in lock service to show workers are alive - also add wall clock time, to handle long running tx, workers periodically update their wall clock time


**Timestamp Oracle**:
1. To provide timestamp to all the nodes, for snapshot isolation
2. batch several RPC calls to oracle
3. needs to give timestamps in increasing order
4. if fails -> recover and issue timestamps later than the last issued one

**Notifications:**
1. Triggers for running services when row data changes
2. Atmost one observer's tx will be committed for each change of an observed column
3. message collapsing - through notify and acknowledgement column for each observer