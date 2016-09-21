# Merge sort, quicksort and median

## Bubble sort

Naive sorting algorithm that works by running through the list and comparing every two neighbouring elements -- if the elements are in the wrong order, they are swapped. Repeat until an entire iteration has been completed without any swaps.

Every pass puts one elements in its proper place, and reduces the instance size by 1.

Bubble sort can be done *in place*, meaning that no extra memory is needed. The sorting is done *in place* in the input array.

## Insertion sort

Build a sorted sublist from left to right. Take the leftmost unsorted element and insert in the correct position in the sorted sublist. To find the correct position, binary search *can* be used, but this will still result in a lot of swapping if the sorting is to be done in place.

## Merge sort

Sorting algorithm in guaranteed $O(n*log(n))$ time. Can not be done in place, and requires $O(n)$ extra space.

## Center of a point set on the line

Given $n$ points on the real line, compute a point $x$ so that the sum of distances to all given points are as small as possible.

## Selection and median finding

Given a set of $n$ elements and an integer $k$, output the element of rank $k$, that is, the $k$:th smallest element.

The median problem is a special case of this problem, where $k=n/2$.

* Choose a random *splitter* $s$ (like in quicksort) and compare it to all elements in $O(n)$ time to get the rank $r$ of $s$.
* If $r > k$ then throw out $s$ and all elements larger than $s$. Repeat.
* If $r < k$ then throw out $s$ and all elements smaller than $s$, and set $k = k - r$. Repeat.
* If $r = k$ then return $s$.

The average time complexity of this algorithm is $O(n)$, but the worst case could still be $O(n^2)$.

There exists a deterministic $O(n)$ algorithm for this problem, but it is complicated and has a very large hidden constant. In practice, the random algorithm is faster.
