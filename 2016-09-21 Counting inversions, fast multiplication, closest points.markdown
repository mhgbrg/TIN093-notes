# Counting inversions, fast multiplication, closest points

Divide-and-conquer.

## Counting inversions

Given a sequence `a1, ..., an` of elements where an order relation is defined, count the *inversions* in the sequence. An inversion is a pair of elements `(ai, aj)` where `ai > aj` and `i < j` (essentially a pair of elements that are in the wrong order).

For example, in the sequence `(2, 4, 1, 3, 5)` the inversions are `(2, 1)`, `(4, 1)` and `(4, 3)`.

The number of inversions can be thought of as a "degree of unsortedness" of a sequence. You can also use it to compare how similar two sequences with the same elements in different orders are.

An example for where this can be used is in a music site that matches people together by their music tastes. Say that you have a ranked list of songs, and another user has a ranked list of the same songs. If the number of inversions is low between your and the users lists, you probably have similar music tastes.

The obvious way of counting inversions is by using brute force. Simple check each pair of elements. This can be done in `O(n^2)` time. With divide-and-conquer we can do better.

The algorithm for this is almost exactly the same as merge sort. The only addition is that we when merging lists also count the number of inversions. When merging two lists `A` and `B`, where `A > B`, taking an element from `A` doesn't result in an inversion, but taking an element from `B` does. How many inversions do you ask? **As many as there are elements left in `A`.**

This is only a constant time addition to the merge sort algorithm, so the time complexity for this algorithm is still `O(n*log(n))`.

## Closest pair of points

Given `n` points `p1, ..., pn` in the plane, find the pair of points that are closest together.

1. Sort the points by their x-coordinates.
2. Take the median `z` of all x-values and put all points with coordinate `x < z` and `x > z` in two sets, `S1` and `S2`, respectively. This can be thought of as drawing a vertical line `L` through the plane, with `n/2` points on each side of the line.
3. Compute the closest pairs in both `S1` and `S2` recursively. Let `d` be the minimum of these two distances.
4. Compute the closest pair where one point is in `S1` and one point is in `S2`. You only have to consider points that are closer than `d` from `L`. To do this even more efficient, we sort the points that are closer than `d` from `L` by their y-coordinates. Now we only need to check distances between two points where the distance in y-axis is less than `d`. This can be done in `O(n)` time.

## Multiplication of large integers

As mentioned earlier in the course, multiplying two numbers with `n` digits with the standard multiplication has a time complexity of `O(n^2)`. Can we do better with divide-and-conquer?

Split the decimal representations of the factors into two halves, and use the distributive law to multiply them. This will result in 4 recursive calls and some additions, and a time complexity of `T(n) = 4 * T(n/2) + O(n)`. Using the master theorem we get the following asymptotic time complexity: `O(n^log2(4)) = O(n^2)`. No improvement.

If we can reduce the number of recursive calls to 3, we can reduce the time complexity. We can do this with some mathematical mumbo jumbo.

This method of multiplication is only more efficient for numbers with more than 100 digits, due to the overhead of the recursive calls.

## Graphs

### Clique

Find a clique of maximum size.

A *clique* is a subset `K` of nodes such that there is an edge between any two nodes in `K`. It is essentially a part of the graph where all nodes are connected to each other.

### Independent set

Find an independent set of maximum size.

An *independent set* is a subset of nodes such that no two nodes are joined by an edge. For example the edges might represent conflicts, and we want to select as many items without conflicts.

### Vertex cover

Find a vertex cover of minimum size.

A *vertex cover* is a subset of nodes such that every edge of `G` has at least one of its two nodes in it.
