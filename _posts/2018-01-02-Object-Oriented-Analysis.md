---
layout: post
title: Object-Oriented Analysis and Design Principles
published: true
date: 2018-01-02
tag: [object-oriented]
---

### Introduction
Object-oriented analysis is a systematic way to tackle real-world problems using a software 
solution. In this blog, I would like to introduce a step-by-step procedure on
how to analyze a real world problem by understanding it, breaking it into small
pieces and solving them one by one.  
I learned most of it from [this](https://www.amazon.com/Head-First-Object-Oriented-Analysis-Design/dp/0596008678/ref=sr_1_1?s=books&ie=UTF8&qid=1514921957&sr=1-1&keywords=head+first+Object-oriented+analysis+and+design)
book and I highly recommend it.

### How to Analyze a Problem
#### Feature List
The very first thing is to understand what problem the customer what to use the
system to solve.  The set of things we want to achieve are also known as
"features". Features are bird view of things both customers and programmers can
understand.

#### Use Case Diagram
Once we understand what we want to achieve, we need to understand how the
system will be used.  More explicitly, what are the **external actors**?
External actors interact with the system but not part of the system. The actors
could be real human users, but they could also be another software. We also
need to make sure the components are sufficient for solving the problems. We
usually use a *use case diagram* to reflect the various usage. For example, in
a route finding software of a subway system, it needs to response passengers'
queries and provide an interface for administrators to maintain the system
(adding, closing stations and so on).

```
(customer)          |=======================|            (admin)
  --|--  -------->  | getRoute(A, B)        |             --|--
    |               | editNetwork()         |  <-------     |
   / \              |=======================|              / \
```

#### Big Components
We then need to lay out the big components (packages) of the system. A classic
example is the model-view-control
[MVC](https://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93controller)
pattern. This helps us to break a big problem into smaller ones, and enables
multiple teams working on the different parts simultaneously. As for the subway
route-finding example, we could break it into three components, 1) Back-end
Information component to store the subway lines, stations, time table and
supporting editing from admin; 2) Route Finding component, for implementing
path finding algorithm, like Dijkstra, using various criteria if needed, such
as time cost, monetary cost, or even the number of line changes. 3) Front-end
information display, which can be visual or step-by-step directions.

#### Requirements and Use Case
**Use Case** is a detailed walk-through of what the system does to accomplish a
specific goal.  It has a specific start and end. It usually has branches and
alternative paths (scenarios).  On the other hand, **requirements** is a list of
detailed functionalities the system provide.  A use case may use multiple
functionalities in the requirements. Some functionalities in the requirements
might be too fundamental to appear in any use case. Requirements and Use Case 
are the two different points of view of our system. 


#### Development Iteration
Now it is the time to design our system, piece by piece (use case by use case).
The classes are usually the nouns in the use case, except the external actors we 
identified earlier. The functions are usually the verbs in the use case. However,
these are not always true, but serve a good starting point. Keeping design principles 
in mind is usually not a bad idea at this stage. In the next post, I will introduce 
some most useful OO principles (in my humble opinion).

