---
layout: post
title: Object-Oriented Design Principles
published: true
date: 2018-01-02
tag: [object-oriented]
---

### Introduction
When I first learned the object-oriented programming (OOP), I was offered
simple examples such as a `Dog` *is an* `Animal` and so does a `Cat`, or a
`Car` *has an* `Engine` and four `Wheels`s, where the former is an _is-a_
relationship indicating *inheritance* and the latter is a _has-a_ relationship
indicating *composition*.  Although these are perfect examples for teaching OOP
concepts, they are far from the principles of designing good OO system.  The
most important criterion of a good OO design is the *flexibility*: how
troublesome, for example, the number of class files we need to touch, if we
want to make a certain change.  Object-oriented design principles are several
guidelines to help us make our software flexible. In this blog, I would like to
introduce several design principles I learned from
[this](https://www.amazon.com/Head-First-Object-Oriented-Analysis-Design/dp/0596008678/ref=sr_1_1?s=books&ie=UTF8&qid=1514921957&sr=1-1&keywords=head+first+Object-oriented+analysis+and+design)
 book.

### Encapsulation 
There are several principles are around the concept of **Encapsulation**.
The first one is that "each individual class does only one thing". This implies that
once we need to make a change of the code, we probably only need to make a change at one place.
Especially, we would like to encapsulate what might vary in future.  The
"Open-Closed Principle" states that classes should be _open_ for extension and
_closed_ for modification. The closeness is achieved by encapsulation, and the
openness is achieved by abstraction (inheritance and composition). A good sign
of needing encapsulation is when one has an urge of copy-pasting code.

### Abstraction
**Abstraction** indicates relationships and interactions among classes. 
Making these relationships and interactions (behaviors) abstract, rather than concrete,
can maximize the flexibility of the system. There are three principles:

* Program to interfaces, not implementation
* Favor composition over inheritance
* The Liskov Substitution Principle (LSP)

The "program to interfaces" is easy to understand. If `classA` needs `classB`
to do something, `classA` should care the result rather than the implementation
of `classB`.  If we find a better approach in `classB`, we don't need to change
`classA` at all. A good example of using this principle is the `stratergy pattern`.

The "favor composition over inheritance" principle is a little subtle. Apparently, 
we can prevent code duplication either way. The difference is that, composition is 
dynamic: one can change the behavior of a class at runtime; whereas inheritance is 
more rigid. Especially, when the base class and the subclass have conflicting behaviors. 
These might not be very obvious during the first design of the system, but could become a 
prominent problem when changes happen. These lead to the third principle: the "Liskov 
Substitution Principle".

[Barbara Liskov](https://en.wikipedia.org/wiki/Barbara_Liskov) received the 2008
Turing Award for her contribution to the design of programming languages. The
"substitution principle" states that "when you inherit from a base class, you must be
able to *substitute* your subclass for that base class." We should always apply
this principle whenever we want to make a subclass. Especially, when we change
our code to accommodate new requirements, we should double check the substitution
principle still hold.  Two alternative ways besides inheritance are
**Delegation** and **Composition**.  Delegation is to another classes as it is.
Composition allows you to use a family of other classes. The difference between
composition and aggregation is very subtle: composition means the host class is
responsible of the destruction of the other classes, where the aggregation does
not. For example, in the observer pattern, the observer reference in the
observable class exists by itself. This is an aggregation. 

(TODO: I hope to write how various OO patterns utilize OO principles in my next post.)
