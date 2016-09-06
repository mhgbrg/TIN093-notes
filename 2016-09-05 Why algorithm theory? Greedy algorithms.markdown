# Why algorithm theory? Greedy algorithms

Problems: Interval Scheduling, Interval Partitioning

## Interval Scheduling

(See problem statement [here](http://www.cse.chalmers.se/edu/course/TIN093_Algorithms_LP1/alg1.pdf))

Can be solved by brute force by testing all possible combinations and picking the best one. This is called *exhaustive search* and can be solved in `O(n*2^n)` time (each interval can either be selected or not).

A myth is that fast algorithms are not needed, because modern hardware is so fast. This is true for inputs of small sizes, but as input sizes grow it is important to have efficient algorithms. A good algorithm often has *polynomial time complexity*.

A way to make the exhaustive search algorithm a bit more efficient is to discard all overlapping intervals once you have chosen an interval for your set. Strictly put, if we put `x=[si, fi]` in `X`, we can delete all intervals that intersect `x`. This makes the amount of branches to the problem a lot smaller, but there are still many branches. Can we come up with an algorithm that has no branches at all?

### Attempt 1

    Put [si, fi] with smallest si in X

This results in problems when the interval with the smallest si is very long. Example:

    [-------------------]
     [-][-][-][-][-][-]

With the above algorithm we would choose the large interval and disregard all the small ones, resulting in a non-optimal solution.

### Attempt 2

    Put [si, fi] with smallest fi-si in X

Counterexample:

    [------][------]
          [--]

Here the small interval blocks the two larger ones, resulting in a non-optimal solution.

### Attempt 3

    Put some [si, fi] in X which intersects the smallest number of other intervals

Counterexample:

    [----][----][----][----]
        [--]  [--]  [--]
        [--]        [--]
        [--]        [--]

### Attempt 4

**Earliest End First: (EEF)**

    Sort intervals such that f1 < f2 < ... < fn
    Put [si, fi] with smallest fi in X
    Remove all intersecting intervals
    Iterate

Works in every case! Though even if we can't find any counterexamples, that is not enough to prove that the algorithm is correct. We have to prove correctness!

#### Proving it!

**Claim**: There exists some optimal solution `Y` containing `[s1, f1]=x` (the intervals are sorted, so `[s1, f1]` is the interval that ends first). If the claim is true, our algorithm is correct.

    Claim => EEF correct (induction on n -- the number of intervals)

**Proof of claim**: `Y` optimal, `X` not in `Y`.

    [---][---][---][---][---] Y
    ---] x

By definition the first element in `X` must end somewhere between the start and end of the first element in `Y`. `x` intersects exactly one `y` in `Y`. This gives us that `(Y\{y})U{x}` is another optimal solution. We simply replace `y`, the element that `x` intersects with, with `x` in the solution.

#### Time analysis

A bare bones EEF runs in `O(n^2)` time. This algorithm simply goes through all intervals on each iteration to find the one that ends first. Here's how we make it faster:

* 2 copies of each interval.
* Put them into two (doubly linked) lists (one that is ordered by start time and one that is ordered by end time).
* Connect copies by pointers.
* Find smallest `fi` in `O(1)` time.
* Intersecting intervals `[sj, fj]` are those with `sj < fi`. To find all intersecting intervals we can simply iterate from the beginning of the first list and remove until we find an element where `s > fi`.

This results in `O(n)` time in total + "time for sorting" (which can be done linearly for some inputs). This is optimal since we always have to consider each and every interval.

This algorithm (and all the previous attempts) are **greedy algorithms**. What this means is that the algorithm will make a local optimal choice in each step, without taking the global scope into consideration. Greedy algorithms can often not be used to find optimal solutions in every case, but for the Interval Scheduling problem it actually works!

### Extended problem

Can the algorithm be extended to handle a similar problem, only where all intervals have a weight and we want to optimize the total weight of all intervals that we take?

## Interval Partitioning

(See problem statement [here](http://www.cse.chalmers.se/edu/course/TIN093_Algorithms_LP1/alg1.pdf))

With Interval Scheduling, we wanted to serve as many requests as possible, an thus wanted to minimize time spent being busy. We did this by always taking the interval that ended first. In this problem we have to serve every request, and so it makes sense to instead minimize time spent being idle. We do this by always taking the interval that starts first.

    Sort the intervals such that s1 < s2 < ... < sn
    for all i: Xi = empty set
    for j = 1 to n
        put [sj, fj] in Xi where i is the smallest index such that [sj, fj] does not intersect other intervals in Xi

`d` = max number of intervals that share a point. Every algorithms needs >= sets `Xi != empty set`. The algorithm uses only d sets:

        sj   fj
        [----]
    -----]
    ------]
    -------]
    --------]

<= `d-1` intervals intersect `[sj, fj]`
