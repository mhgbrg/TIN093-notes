# Introduction

## Problems

A **problem** is that given some input, you want to find out or compute something. A **problem instance** is a specific instance of a problem, with set input values.

Example problem: Given: x, y (integers) Compute: x+y

An instance of this problem could be given 2 and 5, compute 2+5=7.

Specifications for some problems are released before the lectures. **Read the problem specifications the day before the lecture!** (Sunday and Tuesday)

## Algorithms

**Algorithms** are used to solve problems. A problem has infinitely many instances (of course the input parameters can be limited by some constraint), and is essentially a function -- it takes some input values and returns some other value.

An algorithm:

- Is an instruction for how to solve a problem
- Is unambiguous (it should be absolutely clear in each step what the next step is)
- Should require no human intervention
- Consists of work split into small, simple steps

You can run algorithms by hand, but the ultimate goal is to delegate the work to a computer.

The above definition of an algorithm is by no means formal. There exists formal definitions, but they aren't really necessary for developing algorithms. They can however be useful when proving that it doesn't exist an algorithm for a problem, for example the *halting problem*.

Developing algorithms is **not** programming. An algorithm is not a program, it is more of a mathematical definition. **A program is an implementation of an algorithm in the real world.** When developing algorithms you only look at the problem statement and try to figure out a (mathematical) way to solve it. Only when you have a solution, you can write a program that executes it.

Algorithms (and computer science) far precedes computers.

Code should not be used to explain algorithms. You can explain algorithms verbally, but verbal descriptions are often ambiguous and can be misinterpreted. The best way to describe algorithms is with **pseudocode**, and to fill in any details verbally.

## Time complexity

What is faster, addition or multiplication? x+y or x*y?

An algorithms efficiency is measured in time. Memory usage is also important, but memory usage is often limited by time since it takes time to read and write from memory.

**The time complexity of an algorithm is its running time for every instance.** It can be described with a function `t: set of instances -> R+`, but this is far too complicated and not used in reality. This function gives the exact running time for each problem instance, but it is often good enough to know the worst time an algorithm requires to be completed (for any input). A function for this is written as `t: N -> R+`, which describes the maximal running time for instances of length `n`. This is called *worst case analysis*. You can also do *average case analysis*, but its more complicated and often not needed (at least not in this course).

Running time is not measured in seconds, since the actual running time depends on the machine that executes the algorithm. This can't be measured when you only have a mathematical definition. You instead count *elementary operations*, which are just small pieces of work (remember that an algorithm is work split into small simple steps?). Exactly what operations that can be considered elementary depends on the algorithm. If you are developing an algorithm for adding two numbers, you are of course interested in all needed low level operations. If you are however developing a higher level algorithm you can consider addition of two numbers as an operation that takes constant time.

We also ignore constant factors.

The notation used is called big-O. This measures the growth rate of an algorithm as the input size increases.

## Big-O analysis of addition and multiplication

`x`, `y`: integers with `n` digits. Addition of single integers is an elementary operation.

We can compute `x+y` in `O(n)` time by adding each column of digits. This will result in n additions of 2 or 3 digits. Since we remove constant factors, we get `O(n)`. This is the optimal time since we have to read every digit in the two numbers (which takes `O(n)` time). We have to do this since the result depends on every single digit, so we can't skip anything.

We can add `m` integers with `n` digits in `O(m*n)` time. This is not that easy to see, since each addition can result in a carry over with `log(m)` digits. We then get `O(m*n*log(m))`. `O(m*n)` is the correct answer, but it is harder to prove. `O(m*n)` is the optimal time since, like in the previous example, the result depends on every digit.

We can compute `x*y` in `O(n^2)` time by using the standard multiplication algorithm. Multiplying `x` with each digit in `y` will result in n additions of number with length of `O(n)`. Using the result from the previous example we know that this can be done in `O(n^2)` time. Though this is not the optimal solution! (We will come back to this later in the course).
