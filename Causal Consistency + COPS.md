Captures potential causal relationships between operations and guarantees that all processes observe causally related operations in order.

Here is an example of causal consistency.

Causal relations are respected in the following event sequence:

|   |   |   |   |   |   |
|---|---|---|---|---|---|
|P1 :|W(x)1||W(x)3|||
|P2 :|R(x)1|W(x)2||||
|P3 :|R(x)1|||R(x)3|R(x)2|
|P4 :|R(x)1|||R(x)2|R(x)3|

Process P2 observes, reads, the earlier write W(x)1 that is done by process P1. Therefore, the two writes W(x)1 and W(x)2 are causally related. Under causal consistency, every process observes W(x)1 first, before observing W(x)2. Notice that the two write operations W(x)2 and W(x)3, with no intervening read operations, are concurrent, and processes P3 and P4 observe (read) them in different orders.

Following pairs are potentially causally related:
1. a read followed by a later write by the same processor
2. a write followed by a later read by the same processor
3. a transitive closure of the above two types of pairs of operations

The above sequence is disallowed by sequential consistency:
Because the operations cannot be arranged in some sequential order



From the Spanner paper we can see that local reads (read only txs) has been achieved, but r/w txs still require some sort of 2PC protocol and relies on paxos groups across data centers for consistency. Can we do fast local writes?

requirements across datacenters:
1. fast local reads
2. fast local writes


strawman 1:
1. local write to a datacenter shard
2. queue for outstanding writes to other replicas
3. read heavy / write heavy systems
4. equivalent -> dynamo, cassandra
5. no ordering guarantee


Example:

We have a social media app - support for albums (private / public) with photos

C1:   put(photo)    put(album, photo)

C2:                                                       get(album)    get(album, photo)

Anomaly: In an eventually consistent system, the put(album) can arrive later than the first put(photo)

Conflict resolution for most recent value: notion of version numbers? wall clock time? - not synchronized clocks

conflict resolution using vector clocks:
- lamport clocks:
	- tmax highest v# seen from anywhere
	- v# = t = max(tmax+1, wall clock time)
	- how to resolve concurrent writes to the same key? (drawback of weakly consistent systems - require sophisticated conflict resolution protocols)
	- last writer wins (set union doesnt work...)
	- conflict resolution
	- atomic operations ?
- sync calls?

strawman 2:

sink operator

sync(k, v#) -> waits until all datacenters have atleast that version 

put(k , v) -> v#


C1: v# = put(photo),    sync(photo, v#),     put(album, photo)

C2:                                                                                               get(album), get(photo)


- one datacenter goes down  -> sync call blocks
- therefore puts and gets consults a quorum of datacenter



encode order to tell users?
- logging approach? -> designated log server at each datacenter to send requests to other servers instead of communicating independently with the other shard servers. Send logs in log order, which preservers write order. single point of failure. performance degradation. bottleneck log server, receiving log server


**What COPS does**

- Accumulate information of the order of doing things
- 
****
Client context to rescue...


		                                        Context
get(x) ->v2                                 x:v2
get(y) ->v4                                 x:v2, y:v4
put(z, -) ->v3                              put(z, -, x:v2, y:v4)
                                                      |
                                                      |
dependencies-----------------------


- servers consult other shards -> what is the latest version of the dependecny variables. If they are up-to-date, only then next operations can be applied
- else hold onto update
- cascading dependencies can slow down the system quite a lot -> problem with causal consistency
- after put context is replaced by the v# for the latest put
- 
