---
layout: post
title: The Stupidest Miskates I Have Made
published: true
date: 2017-03-14
tag: [fun]
---

<script src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script>

`title += "So Far..."`

Here is a list of the stupidest mistakes I have made so far. 
So, from time to time, I can look at them and laugh at myself. :P


**NOTE: all codes are mistakes!**

### How to convert a `char` to an `int` in C++
{% highlight cpp %}
char c = '1';
int  a = int(c);
assert( a != 1 );
{% endhighlight %}

### How to give a pointer a `new` content in C++
{% highlight cpp %}
int * ptr = nullptr;
void foo( int* ptr ){
  ptr = new int(1);
}
assert( ptr == nullptr );
{% endhighlight %}


