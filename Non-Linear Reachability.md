Types of Non-Linear Reachability Methods:

1. Conservative Linearization
2. Conservative Polynomialization
3. Picard Iteration


Conservative Linearization:
- Linearize the non linear dynamic function at linearization points x* and u*
- compute the reachable set for the linearized system using reachability algos for linear systems where the set of linearization errors L is treated as an additional uncertain input
- linearization points are the estimated center of the time interval reachable sets X(T)
- Calculate L using mutual dependence ( start with estimate L=0 and then iteratively increase L by a factor of lambda until convergence)
- advantages:
	- one can use the same reachability analysis algs as linear systems
	- alg is quite fast
- disadv:
	- accuracy of reachable set enclosure is quite low - for strongly non linear systems or large input set
	- fails to capture non-convexity of reachable sets of non-linear systems

 Conservative Polynomialization:
 - uses polynomial instead of linear approximation of the function
 - computed using a Taylor series expansion
 - polynomial zonotope for non-convex set rep - for tight enclosures of reachable sets
 - advantages:
	 - computes very accurate non-convex set rep
	 - can handle highly non-linear systems + large initial sets
- disadv:
	 - significantly slower than conservative linearization


Picard Iteration:
