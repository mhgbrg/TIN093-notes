# Reductions, NP-completeness

## Reductions

How would a mathematician prepare tea? He would use an algorithm! He pours water in a boiler, boils the water, and then pours it in a cup. What if there's already water in the boiler? He pours it out, and uses the same algorithm! He has now *reduced* the problem to a problem that he has the solution to.

Computing `a*b`, where `a` and `b` are integers, can be reduced to squaring a number.

Suppose that we have an algorithm that can square an integer in `O(S(n))` time, where `S = polynomial`, `S(n) >= n`.

We can transform `a*b` to: `a*b = ((a+b)^2 - (a-b)^2)*1/4`. Now we have addition, subtraction and squaring, but we also have a multiplication. Wasn't the goal to get rid of this multiplication? Well, the `1/4` here is a constant. **It doesn't depend on the input**. This means that we can discard that we have a multiplication, and we have reduced the problem to adding, subtracting and squaring. This calculation can be done in `O(S(n))` -- the same as the squaring. This is because the other calculations can be done in linear time. We can thus say that the problem of multiplication has been reduced to the problem of squaring.

Is squaring easier than multiplication? No! If it was, we couldn't reduce multiplication to squaring. Given any algorithm for squaring, we can reduce multiplication to the same time bound. This means that squaring can't be easier.

### Two uses of reductions

* Solve a problem X with the help of an existing algorithm for a problem Y.
* Prove that Y is not easier than X.

### Definition of reduction

`X, Y` = problems. `|x|` = length of an instance `x` of `X`. "`X` is reducible to `Y` in `O(t(n))` time" means:

    We can transform every x (|x| = n) into an instance y = f(x) of Y, and transform a solution of y back to a solution of x -- everything in t(n) time.

So basically it takes `t(n)` time to both transform the problem into a harder problem, and to transform the solution from that computation back again to the original problem. Note that this only includes the transformations, and not the solving of the problem.

Here's an example:

Assume that `Y` is solvable in `O(u(n))` time. A reduction of `X` to `Y` will look like this:

    x ---> y=f(x) ------> solution of y ---> solution of x
      t(n)        u(t(n))               t(n)

The step from `y` to a solution of `y` is `u(t(n))`, since in the worst case the transformation from `x` to `y` will produce `t(n)` symbols. It is therefore not enough to say that `y` can be solved in `u(n)` time.

    Y easy => X easy
    X hard => Y hard

## Optimization vs decision problems

**Optimization problem**: Given an instance x, find a solution with min/max value.

**Decision problem**: Given an instance x and a number k, does x possess a solution with value `<= k` / `>= k`.

If we can solve the optimization problem, we can also solve the decision problem. We can also use the decision problem to find the optimal value, for example by trying all values of `k`. The decision problem is formally easier to solve, and it is often easier to compare decision problems than optimization problems.

`X, Y` = **decision** problems. `|x|` = length of an instance `x` of `X`. "`X` is reducible to `Y` in `O(t(n))` time" means:

    We can transform every x into an instance y = f(x) of Y such that x answers Yes <=> y answers Yes, and f is computable in O(t(n)) time.

"`X` is reducible to `Y` in polynomial time" is the same as above, just that `t` is a polynomial. This is written as `X <=p Y`.

## Easy vs. hard problems

`P` = the class of decision problems that can be solved in polynomial time. If `X` is reducible to `Y` in polynomial time, and `Y` is in `P`, we know that `X` is also in `P`. This is rather obvious.

**Proof**: `X --f--> Y`. `f` computable in `p(n)` time. `Y` solvable in `q(n)` time. `p, q` are polynomials. To solve `x`, we compute `f(x)` and solve this instance. The time it takes is: `p(n) + q(p(n))`, which is polynomial.

`NP` = the class of decision problems for which solutions can be verified in polynomial time. `P` is a subset of `NP`.
