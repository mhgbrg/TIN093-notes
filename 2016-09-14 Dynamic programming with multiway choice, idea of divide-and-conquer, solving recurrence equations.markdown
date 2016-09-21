# Dynamic programming with multiway choice, idea of divide-and-conquer, solving recurrence equations

## Segmentation

A.k.a. linear regression. Fit a number of straight lines to a number of points, so that the difference between the points and the lines are as small as possible.

$e_{ij}$ penalty (or quality score) for $(x_i, \ldots, x_j)$. Find a segmentation that minimizes the sum of penalties. You also have a constant penalty for every added segment.

This problem is solved with dynamic programming.

$OPT(j) :=$ min. sum of penalties of a segmentation of this prefix.

$OPT(j) = \min_{i \leq j} \{ e_{ij} + OPT(i - 1) \}$.

The time complexity for this algorithm is $O(n^2)$, since to compute the optimal value for $j$ we have to compare the optimal values for all $i \leq j$.

### Alternative algorithm

(Only works for the "max" version)

Consider $(x_i, \ldots, x_j)$ as interval with weight $e_{ij}$. Solve this instance of Weighted Interval Scheduling.

We don't have to develop a new algorithm, but can instead use an algorithm for another problem that we already have an algorithm for. This is called "reduction", the process of reducing a problem of one type to another problem.

The time complexity for this algorithm is still $O(n^2)$. This might be surprising, since the time complexity for the Weighted Interval Scheduling algorithm is $O(n)$, but it is important to notice that this $n$ is the number of intervals given, which is different from the $n$ given in our problem, which is the number of points given.

## Searching

We are given a set $S = \{s_1, \ldots, s_n\}$ which are already sorted, $s_1 < \ldots < s_n$. The problem is to decide whether or not $x \in S$, and if so, where it is.

    Search(i, j):
        if i = j:
            return i
        else:
            m = (i + j) / 2
            if x <= Sm:
                Search(i, m)
            else:
                Search(m + 1, j)
            end
        end
    end

This is called *binary/bisecting search* or *halving strategy* and the time complexity of it is $O(log(n))$.

$T(n): time for instances of length $n$.

$T(n) = T(n/2) + O(1)$

$T(1) = O(1)$

$T(n) = O(log(n))$

Equations like this are called *recurrence equations*.

## Skyline

**Instance**: Described by $(l_i, r_i, h_i), i=1, \ldots, n$.

**Output**: Described by the sequence of heights and points on the x-axis where the height changes.

We try to solve this problem by using an incremental approach.

Insert rectangles one by one and update the skyline. $O(j)$ time for inserting $j$-th rectangle, resulting in $O(n^2)$ time in total.

$T(n) = T(n-1) + O(n)$

This is not good! A better approach would be to:

* Divide the instance arbitrarily in two instances with $\frac{n}{2}$ rectangles.
* Compute their skylines recursively.
* Merge them.

(Same approach as merge sort)

$T(n) = 2 * T(\frac{n}{2}) + O(n)$

$T(n) = O(n*log(n))$

## Now for some mathematics! Solving recurrence equations

$T(n) = a*T(\frac{n}{b}) + c*n^k$

$a > b^k : O(n^{log_b(a)})$

$a = b^k : O(n^k*log(n))$

$a < b^k : O(n^k)$
