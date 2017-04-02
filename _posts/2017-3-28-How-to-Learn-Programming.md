---
layout: post
title: How to Learn Programming 
author: Yihui (Ray) Ren
published: true
date: 2017-03-28
tag: [tutorial]
---

Recently, a graduate student asked me for advice on how to improve programming skills.
My immediate response in my mind is that "You asked the wrong person a right question".
But I think he will feel bad if I do not try to respond. So, here it is...
Note that, this guidance is biased toward to a scientific computing rather than
general software engineering. But I think the basic things like data
structures and algorithms are common. 

## Need Some Editing ...
This article probably can use some editing. For example, instead of ordered by topics, 
it might be more friendly if it is ordered by levels. I also want to add links to 
the jargons that I throw from time to time. And many more. Once I find some time...

## Understand the Hardware
Programming is about writing programs to let computer perform tasks and solve
problems.  One should first understand what a modern computer architecture
looks like.  How does CPU compute? What are registers, instructions (i.e.
AVX-512) and caches (i.e. L1, L2)? How are data stored and passed to CPU? What
is caching and cache miss?  What is concurrency and data race?  If you are
interested in high performance computing using co-processors, you may also want
to know the architectures of Nvidia GPUs and Intel Xeon-Phi.  Understanding the
hardware is very crucial for writing efficient code. 

## Data Structures and Algorithms
The book title "Algorithms + Data Structures = Programs" explains quite well
what a computer programs are. I will discuss data structure and algorithm one 
by one, but please keep in mind that these two are not separable at all.

### Data Structures
*Data structures* are ways to store data to make some fundamental operations more
efficient. Common fundamental operations include, but not limited to, searching for 
particular data , inserting data, iterating through all the data,
and deleting some data.  The most often used data structures are
array (vector), list, binary tree (set), hash table (unordered-map).  Queue,
stack and heap (priority-queue) are also very useful, but they are more of 
concepts than implementations.  One exercise could be to implement queue, stack
and heap using an array or a list.  One should definitely know the
pros and cons about array, list, binary tree and hash map in terms of the time
complexity of each fundamental operation. For example, if the algorithm requires 
adding and removing data frequently, should an array or a list be used? Then,
if needed, one can learn some advanced data structures, such as red-black tree,
AVL tree, KD tree, segment tree, interval tree, prefix tree (trie),
binary-indexed (Fenwick) tree, disjoint-set (union-find), adjacent list
(graph).  The list can go on and on.  But the good news is that it is not necessary 
to learn all of them. The advanced structures usually are used in more advanced
algorithms, which does a some very specific task. For example, the Fibonacci
heap, usually used in the Dijkstra's shortest path algorithm in a non-negative
weighted graph, has better performance in theory, but only when the graph is
very dense (high ratio between the number of edges and the number of nodes).
I recommend do not throughly learn all above data structures one by one, but check 
them out casually to see what kind of operations they support and the
corresponding time complexity for each operation.  So, once one is designing an
algorithm to solve a problem, she/he knows which data structure might be
useful. For example, to find a connected component of a graph given an edge
list, instead of storing the graph as a graph data structure and perform
BFS/DFS on it, using a disjoint-set data structure will solve this problem
elegantly.

### Algorithms
*Algorithms* are ways to solve a problem using a computer.  They are sets of
instructions for a computer to perform certain tasks.  The tasks may include
search and retrieve data, which closely relates to *data structures*.  There
are no exhaustive lists of all algorithms, but some categories of algorithms
that cover most of algorithms. I would recommend to learn algorithms through a
course.  If you cannot find one on campus, many universities release their
video recordings online.  Coursera is also a good resource. The most important
algorithms are sorting, i.e. quick sort and merge sort, dynamic programming,
i.e. to solve knapsack, rod cutting and longest common substring, greedy
algorithms, i.e. to solve minimum-spanning-tree (MST) and activity selection,
and basic graph algorithms like breath-first-search (BFS) and
depth-first-search (DFS). They are important not only in the practical point of
view, but also important as paradigms for designing new algorithms, i.e.
divide-and-conquer in merge-sort and reducing to subproblems in dynamic
programming.  One should also try to implement these algorithms on their own
without referring to the pseudo-code. More advanced algorithms, like advanced
data structures, are more problem specific. With a solid understanding of the
basic algorithms, to grasp an advanced algorithm should not be a problem. Here
are some more advanced algorithms: Dijkstra's shortest path algorithm, Tarjan's
strongly connected component algorithm, Kahn's topological sort algorithm, Fast
Fourier Transform, and Metropolis-Hastings algorithm. Furthermore, one needs
to be aware of the NP-hard problems.  The discussion of them is beyond the scope
of this article. To put it simple, they are very hard problems to solve, and there are
no algorithms can solve them exactly in polynomial time. For practical reasons,
one should familiarize themselves with such problems and be able to identify
them. 

## The Choice of Programming Languages

"Programming Languages" are interface between programmers and computers. How do
you let the computer read a file, display something on the screen or store a
variable in the memory?  A programming language sets the grammar (syntax) to
allow you to communicate to the computer.  I don't think there is an absolute
the best programming language.  I think the best language depends on the tasks,
problems one wants to solve, and/or the choice of your boss or your colleagues.
Good programming languages usually provide a various sets of data structures
and algorithms, so programmers can use them directly to save time.  C++, Java
and Python are probably the most common out there.  The complete list of pros
and cons could be a book. But here is a short discussion.  C++ is good on
performance, but requires a lot on the programmer's side. For example, the
programmer needs to understand stack, heap and memory allocation/deallocation.
C++ provides means to access memory directly, like C, which is very flexible
but also dangerous if the programmer is not very experienced. Java takes care
of all these via its virtual machine and garbage collection.  The cost is the
performance. But to hire many programmers to collaborate on a large project,
and to prevent any of them to write dangerous code, industries prefer Java
(except ones demanding high performance such as the gaming industry and
financial industry involving high-frequency trading).  Python is very versatile
and nimble.  It is also very easy to learn by oneself.  It takes much less time
to write a python code usually. I think any of these will be a good start.  If
you already know one of these, you probably want to learn a functional
language, such as LISP or Haskell. 

As my work mainly involves high-performance computation and simulation, C++ is
the language of my choice to do such task. For plotting and data analysis, I
use Matlab, Mathematica and Python. Matlab supports matrix nicely. Mathematica
good at symbolic functions. Python has many useful packages. All of them are
capable of visualization. I will spend a little bit time on how to learn C++.
I think there are three phases in learning C++.  First phase is when one is
still studying the fundamental data structures and algorithms.  At this stage,
I would recommend to implement the data structures and algorithms of interests
using pointers in old-fashioned C style.  This will help you understand the
data structures and algorithms deeply. It will also help you expose your
mistakes, i.e. memory leaks, segment fault and so on. The learning process
might be frustrating, but the experience is very rewarding.
[Stackoverflow](https://stackoverflow.com/) is your best friend at this stage.
The second phase is to use C++ as a development tool for simple code.  At this
stage, one should explore the C++ standard template library (STL), and many
common data structures and algorithms already been implemented (by very good
programmers). For example, the common data structures: vector, list, set,
unordered-map, and common algorithms: sort, lower-bound, nth-element,
max-element (in `<algorithm>`).  One should learn how to use them correctly and
efficiently, and pay attention to the time complexity and multi-thread safety.
The third phase is to develop a complex project.  Learn object-oriented
programming and some design patterns. Understand polymorphism both at run time
and at compile time. Learn how to use `template` and `lambda`, especially under
the C++14 (and C++17) standard. If you want, you can learn meta-programming as
well. These techniques will help you modularize a complex project and make
testing, debugging, maintaining and sharing your code with others much easier.

Beyond this point, I think how to use programming as a skill depends on the
personal needs.  For doing research, learning numeric methods, MPI, and
additional libraries in need such as boost, BLAS, CGAL, and so on will help one
write reliable code easier.  I also recommend to participate online coding
competitions, such as [TopCoder](https://www.topcoder.com/),
[HackerRank](https://www.hackerrank.com/) and so on.  You will learn how to
attack a problem, implement your algorithm efficiently; and most importantly,
you will have a chance to see others code and learn from them.  Developing some
tools or joining an open-sourced projects are also very helpful.

Learning programming, just like learning an instrument, takes time and practice.
You have to make mistakes to improve yourself. You should be patient, recover
from frustration quickly and enjoy yourself during the course. Peter Norvig
wrote an excellent article:
["Teach Yourself Programming in Ten Years"](http://norvig.com/21-days.html).


