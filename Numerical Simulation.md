
- Definition of a **Well-posed** problem:
	- solution exists (existence)
	- solution is unique (uniqueness)
	- solution depends continuously on the input (stability)

**Ill-posed** otherwise

- Condition number

output y depends continuously on input x if small perturbations in input causes small changes in the output.


Lipschitz continuity states that for there exists some k for which ||dy|| < k||dx|| for any dx.
K -> Lipschitz constant or condition number
basically an upper bound on the rate of change in output v/s input

- Relative Condition number

Problem is ill condition if k is large for any x, problem is well-conditioned if it is small for all x.



As a computer scientist you want to trust the solutions you get from numerical solutions. The issue is that every computer scientist can represent initial conditions only up to a finite accuracy - any differences below threshold of the datatype cannot be realized.

For systems with no continuous dependence -> we dont trust




Types of ODEs:

1. Linear systems - u' = Au
2. Affine systems - u' = Au + b
3. Linear time varying systems - x' = A(t) + B(t)y
4. Linear time invariant control systems - x' = Ax + Bu
5. Non linear systems - x' = x^2 * y, y' = y
6. Linear parameterized systems x' = Ax, A = \[ \[ 0 1\], \[ -1 p\] \] p in some range



u(t+h) = u(t) + hu'(t)


**Fundamental theory of Numerical Analysis:**


**Convergence** = **Stability** (error due to imprecise input is bounded) + **Consistency** (with exact data error can be made arbitarily small by choosing a small enough step size)





1. Euler method
2. Adam's Bashforth (AB) method


Conditional v/s Unconditional stability:

Backwards euler method being an implicit method is unconditionally stable. It requires more function evaluations  than explicit methods.


1. Implicit Method
2. Runge - Kutta methods (RK1, RK2 and RK4)



Stiff systems -> A system of ODEs is called stiff if it involves rapidly changing components together with slow changing ones. Solvers are forced to take small time steps due to stability issues.

We can approximate points on a trajectory in a given location to an arbitrary precision. When jumps are detected  (outgoing transitions are enabled). Events can be detected as **zero crossing** of suitable functions that are zero on the border.


Root finding algorithms to estimate the exact time of crossing




Zeno behaviour -> switching times can get closer and closer, which can lead to state where an infinite number of switches may occur in finite time. This is called Zeno behaviour. The simulation can get stuck in such situations.



