# Greedy versus dynamic programming

Even slight changes in the problem specification might change the nature of the algorithm completely. One such case is Interval Scheduling vs Weighted Interval Scheduling.

## Weighted Interval Scheduling

(See problem statement [here](http://www.cse.chalmers.se/edu/course/TIN093_Algorithms_LP1/alg2.pdf))

The same problem as Interval Scheduling, with the only addition that each interval has a weight and that we now want to maximize the weight of all taken intervals instead of just the number of intervals. Interval Scheduling is a subproblem of Weighted Interval Scheduling when the weights for all intervals are set to 1. This means that Weighted Interval Scheduling can't be easier than regular Interval Scheduling.

To develop an algorithm for Weighted Interval Scheduling, we might ask ourselves: Can we extend our original solution to regular Interval Scheduling -- EEF -- to also work with weights? Can we develop a new greedy algorithm?

EEF fails, since it doesn't take weights into consideration at all. If we instead take the heaviest intervals first, we might end up choosing an interval that blocks all the other intervals, resulting in a non-optimal solution. There are in fact no greedy algorithm that solves this problem.

### Developing an algorithm

The intervals are sorted by their ending times, meaning that:

    f1 < f2 < f3 < ... < fn

We begin from the left and find the maximum valid solution for each `j`, where `j` is an index between 1 and `n`. We define the following function:

    OPT(j) = max weight achievable by a subset of disjoint intervals from
             {[s1, f1], [s1, f2], ..., [sj, fj]} (the first j intervals)

In the end we want to find `OPT(n)`.

#### Defining OPT(j)

    OPT(1) <= OPT(2) <= ... <= OPT(n)

We define `OPT(j)` by induction on `j`.

**Induction base**: `OPT(1) = v1` (weight of interval that ends first)

**Induction step**: Consider any fixed `j > 1`. Suppose that all `OPT(i), i < j` are already computed. For each interval we have two options: include it in the solution or don't include it.

    OPT(j) = max {
        OPT(j-1)       <-- we don't take [sj, fj]
        vj + OPT(p(j)) <-- we take [sj, fj]
    }

    p(j) = max { i | fi  < sj } <-- auxiliary function

#### Time analysis

* Calculating all `p(j)` can be done in `O(n)` time if all `fi`, `sj` are sorted. <!-- how? -->
* One instance of `O(j)` is done in in constant time (if we consider arithmetic operations with real numbers as elementary). This is done `n` times, resulting in a total time complexity of `O(n)` for `OPT(n)`

#### Implementation

The formula we developed above is recursive, but we don't want to implement it recursively (at least not if we don't know exactly how the compiler works). The reason for this is that it results in a lot of repeated calculations. Consider the following tree for calculating `OPT(6)`:

    OPT(6) - OPT(3) - OPT(1)
    |        |- OPT(2)
    |- OPT(5) - OPT(3) - ...
       |        |- ...
       |- OPT(3) - ...
          |- ...

Just in this small subtree we have to calculate `OPT(3)` three times. We instead want to implement the algorithm iteratively and save the result of each calculated `OPT(j)` in a table.

| j      | 1 | 2 | 3 | 4 | 5 | 6 |
|:-------|:--|:--|:--|:--|:--|:--|
| OPT(j) | 2 | 4 | 6 | 7 | 8 | 8 |

`OPT(n)` only calculates a number, namely the maximum achievable weight. How do we get the subset that resulted in this weight? Instead of just saving the weights of each `OPT(j)`, we also save the set of intervals that make up the weight.

| j      | 1   | 2   | 3     | 4   | 5       | 6       |
|:-------|:----|:----|:------|:----|:--------|:--------|
| OPT(j) | 2   | 4   | 6     | 7   | 8       | 8       |
| set    | {1} | {2} | {1,3} | {4} | {1,3,5} | {1,3,5} |

To do this we have to copy the set of intervals multiple times and add an element, resulting in a time complexity of `O(n^2)`. We can instead store an answer to the question "where did the previous optimal solution come from?", and a flag indicating whether or not we added the interval.

| j      | 1   | 2   | 3   | 4   | 5   | 6  |
|:-------|:----|:----|:----|:----|:----|:---|
| OPT(j) | 2   | 4   | 6   | 7   | 8   | 8  |
| from   | -   | -   | 1   | -   | 3   | 5  |
| added  | yes | yes | yes | yes | yes | no |

We can then build the set for `n` in `O(n)` time by *backtracing*.

This process of continuously saving the result of a previous computation is called *dynamic programming*.

## Dynamic programming

1. Choose parameters that limit the sub-instances (parameter for the recursive function, in this case the first `j` intervals given a list of intervals sorted by end-time).
2. **Define** a recursive function indicating the optimal value of sub-instances (`OPT(j)`).
3. **Compute** this function without recursive calls (iteratively).
4. Time (usually) = size of the table \* time spent on each entry (in this case constant).
5. Get solutions by backtracing from the final value (don't copy partial solutions!).
6. "One scheme fits all" (the same pattern of dynamic programming can be used for many different problems)
7. Prove correctness of the recursive formula (just the function itself, not the entire algorithm).
