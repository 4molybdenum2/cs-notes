Physical timestamps might be inconsistent with causality (ordering in which events might take place)


**Physical clocks**: count no of seconds elapsed
**Logical clocks:** count no of events occurred

capture causal dependencies:
e1 < e2 => T(e1) < T(e2)


Lamport clock algorithm:

start:
	t = 0
	on any event occurred: t = t+1
	request to send message m: t = t+1, send (t,m)
	on receiving (t', m): t = max(t, t') + 1, deliver m to application
end


Properties:
1. a ---> b implies L(a) < L(b)
2. But L(a) < L(b), doesn't imply a ----> b, maybe a and b were concurrent, we just know that b didn't happen before a (partial order)
3. L(a) = L(b), a != b


- To uniquely identify events we can combine the node ID with the timestamp in a pair
	(timestamp(e), nodeid(e)), uniquely identifies event "e".


- Define a total order "--" using Lamport timestamps:

	(a -- b) : (L(a) < L(b)) or (L(a) = L(b) and N(a) < N(b))

This order is causal a --> b equivalent to a -- b



**Vector Clocks**:

Given Lamport clocks we can't say whether one event happens before another or they are concurrent.

To detect those we need vector clocks.
- Assume N nodes in system
- Vector timestamp of event a is V(a) = (t1, ...., tn)
- t_i is the number of events observed by node N_i
- each node has a current vector timestamp T
- on event on a node i, increment vector element at index i
- attach current vector timestamp to message
- receipient merges message vector into local vector




Vector clock algorithm:

start:
	T = (0,0,..........0)
	any event occur on node i: T[i] += 1
	on request send message m, T[i] += 1, send(T, m)
	on receiving message (T', m): T[j] = max(T[j], T'[j]) for all j in (1,n)
	T[i] += 1, deliver m to application
end



T || T' , if T not <= T' and T' not <= T


