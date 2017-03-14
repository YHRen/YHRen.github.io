---
layout: post
title: The Stupidest Miskates I Have Made
published: true
date: 2017-03-14
tag: [fun]
---

<script src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script>

Here is a list of the stupidest mistakes I have made so far. 
So, whenever I am looking at them, I can laugh my ass off :D

{% highlight cpp %}
char c = '1';
int  a = int(c);
// expecting a=1 ? maybe in MATLAB not here.
{% endhighlight %}

{% highlight cpp %}
int * ptr = nullptr;
void foo( int* ptr ){
  ptr = new int(1);
}
// expecting *ptr is 1 ?
{% endhighlight %}


