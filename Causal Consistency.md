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



**COPS**

requirements across datacenters:
1. fast local reads
2. fast local writes

eventual consistency (dynamo, cassandra):
- no order guarantee
- good performance (system doesnt require you to do anything immediately)
- anomalies may arise, due to incorrect ordering of operations on a system

conflict resolution using vector clocks:
- lamport clocks:
	- tmax highest v# seen from anywhere
	- v# = t = max(tmax+1, wall clock time)
- how to resolve concurrent writes to the same key?
	- last writer wins (set union doesnt work...)
	- conflict resolution
- sync calls?


encode order to tell users?
- logging approach?
- append to datacenter log server
- send logs 
- eliminates syncs - regains performance
- drawback - bottleneck log server, receiving log server

Client context to rescue...


		                                        Context
get(x) ->v2                                 x:v2
get(y) ->v4                                 x:v2, y:v4
put(z, -) ->v3                              put(z, -, x:v2, y:v4)
                                                      |
                                                      |
dependencies-----------------------


