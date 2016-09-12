# Dynamic programming, further examples

Problems: Knapsack, subset sum, sequence comparison, segmentation. See problem statements [here](http://www.cse.chalmers.se/edu/course/TIN093_Algorithms_LP1/alg3.pdf).

## Knapsack

* Weights $w_1, w_2, ..., w_n$
* Values $v_1, v_2, ..., v_n$
* Capacity $W$ (integer)
* Find $S \in {1, 2, ..., n}$
* Such that $\sum\limits_{i \in S}{w_i} \leq W$
* and max $\sum\limits_{i \in S}{v_i}$

## Greedy attempt

* Sort items such that $\frac{v_1}{w_1} \leq \ldots \leq \frac{v_n}{w_n}$.
* Take items in this order, put them in the solution until the max weight is filled.

This algorithm doesn't work, since there might be an item that is in the optimal solution, but that won't be picked by this algorithm since the space has already been filled up with smaller weights.

## Subset sum

"Is there a subset of $\{w_1, w_2, ..., w_n\}$ with sum $W$?"

Subset sum is an easier version of the knapsack problem, where the value of all items is the same as the items weight. In this case we are only interested in knowing whether or not a set with sum $W$ exists, which is easier than trying to find the set with the largest sum that is smaller than $W$.

A difference from the interval scheduling problem is that we need two parameters, $j$ and $w$.

$P(j, w) = 1$ if some subset of $\{w_1, ..., w_j\}$ has sum $w$, otherwise $P(j, w) = 0$.

### Computing $P(j, w)$

We compute $P(j, w)$ by using induction. We have the following base cases:

$\forall w > 0 : P(0, w) = 0$

$\forall j : P(j, 0) = 1$

Suppose that we have already computed $P(i, y)$ for all $i < j$, $y < w$.

$P(j, w) = P(j - 1, w) \lor P(j - 1, w-w_j)$

We can try this formula with an easy example, $w_1 = 1, w_2 = 1, w_3 = 5, w_4 = 2, w_5 = 2$:

| w\j | 0 | 1 | 2 | 3 | 4 | 5 |
|:----|:--|:--|:--|:--|:--|:--|
| 0   | 1 | 1 | 1 | 1 | 1 | 1 |
| 1   | 0 | 1 | 1 | 1 | 1 | 1 |
| 2   | 0 | 0 | 1 | 1 | 1 | 1 |
| 3   | 0 | 0 | 0 | 0 | 1 | 1 |
| 4   | 0 | 0 | 0 | 0 | 1 | 1 |
| 5   | 0 | 0 | 0 | 1 | 1 | 1 |
| 6   | 0 | 0 | 0 | 1 | 1 | 1 |
| 7   | 0 | 0 | 0 | 1 | 1 | 1 |
| 8   | 0 | 0 | 0 | 0 | 1 | 1 |

A dynamic programming algorithm in pseudo-code would look something like this:

    Subset-sum(n, W):
        Let A[0 .. n][0 .. W]
        Set A[0][w] = 0 for w > 0
        Set A[j][0] = 1
        For j = 1 .. n:
            For w = 1 .. W:
                A[j][w] = A[j - 1][w] or A[j - 1][w - Weight(j)]
            End
        End
        Return A[n][W]
    End

### Time complexity

There are $n*W$ cells in the table, and each cell is calculated in $O(1)$ time. This gives us the time complexity $O(nW)$. **Though this is not polynomial**. Since $W$ is the *value* of the input, and not the *length* of the input, which would be the number of digits in $W$, this time complexity is called *pseudo-polynomial*. In the worst case this time complexity can be as bad as exponential.

$s = \text{number of digits in W}$

$s = log(W) \Rightarrow W = 2^s \Rightarrow O(nW) = O(n2^s)$

## Optimization version of subset sum

"Find a subset of $\{w_1, ..., w_n\}$ whose sum is $\leq W$ but maximal."

This is the optimization version of the previous problem. Instead of only wanting to know if there is a sum of $W$, we want to find the largest sum smaller than $W$.

The algorithm is very similar to the algorithm for the previous problem. After changing the definition of $OPT$ to match the problem, we only need to replace $\lor$ with $max$ and make sure that we add the weight of the item if we include it.

$OPT(j, w)$: the largest number $\leq w$ which is the sum of some subset of $\{w_1, ..., w_j\}$.

$OPT(j, w) = max \{ OPT(j-1, w), w_j + OPT(j-1, w - w_j) \}$

## Knapsack

We can use the almost exact computation as for the optimization version of subset sum. The only difference is that we add the value of the object instead of its weight.

$OPT(j, w)$: the largest value of a subset of $\{(v_1, w_1), ..., (v_j, w_j)\}$ with weight $\leq w$.

$OPT(j, w) = max \{ OPT(j-1, w), v_j + OPT(j-1, w - w_j) \}$

The time complexity for this algorithm is also $O(nW)$, which is pseudo-polynomial. No algorithm with polynomial time is known to exist for the knapsack problem, and there are reasons to believe that there is no such algorithm (for more info, see [P vs NP](https://en.wikipedia.org/wiki/P_versus_NP_problem)).

## Sequence alignment

"How similar are two words?"

For example, how similar are the words "CREAMY" and "CARAMEL"? To get a sense of this, we see that we can line up the words like this:

    C_REAMY_
    CAR_AMEL
     x x  xx

Where `x` represents a position that doesn't match. It can either be a *mismatch* between two characters, or a *gap* (represented by `_`). You can line up two words in a lot of different ways:

    CREAM_Y
    CARAMEL
     xx  xx

How do we know which one is the "best"? To do this we must introduce a penalty for introducing a gap and a penalty for mismatching two characters. For example matching two different vocals might result in a smaller penalty than matching a vocal and a consonant. How good the entire alignment is is then decided by the total penalty for all gaps and mismatches.

Let $a_1, ..., a_n$ and $b_1, ..., b_m$ denote the first $i$ characters in the first word and the first $j$ characters in the second word.

$OPT(i, j) :=$ minimal number of mismatches in an alignment of $a_1, ..., a_i$ and $b_1, ..., b_j$.

We want $OPT(n, m)$.

### Problem analysis

What can happen to $a_i$?

1) $a_i$ is not matched.


    a1 ... ai_1 ai
    b1 ... bj

2) $a_i$ is aligned to $b_j$.


    a1 ... ai
    b1 ... bj

3) $a_i$ is aligned to $b_k$.


    a1 ... ai
    b1 ... bk ... bj

The last case would require $n$ time, since you would have to match $a_i$ with $b_k$. This is not fast! We instead replace this case to say that $b_j$ is not matched.

    a1 ... ai
    b1 ... bj-1 bj

$OPT(i, j) = min \{ OPT(i-1, j) + g, OPT(i-1, j-1) + f(i, j), OPT(i, j-1) + g \}$

where $g$ is the penalty for gaps and $f(i, j)$ is a function that decides the penalty for mismatching characters $i$ and $j$.

    Sequence-alignment(n, m):
        Let A[0 .. n][0 .. m]
        A[i][0] = g * i where i <= n
        A[0][j] = g * j where j <= m
        For i = 1 .. n:
            For j = 1 .. m:
                A[i][j] = min(
                    A[i-1, j] + g,
                    A[i-1][j-1] + f(i, j),
                    A[i][j-1] + g
                )
            End
        End
        Return A[n][m]
    End

The time complexity for this algorithm is $O(nm)$, which this time is truly polynomial since both $n$ and $m$ represent the size of the input (the length of the two words).

This problem can be encoded as a graph, and solved with a shortest path algorithm (see explanation in course book).
