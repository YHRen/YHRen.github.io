---
layout: post
title: InfiniteSequence topcoder SRM413 div2 level3
published: true
date: 2017-04-01
tag: [topcoder, div2, level3, ]
---

### Introduction
If you have not tried to solve this
[problem](https://community.topcoder.com/stat?c=problem_statement&pm=9837)
by yourself, please do so. Otherwise, this post will make little sense to you.

The statement of the problem is a recursion, $$a_i = a_{[i/p]} + a_{[i/q]}$$,
$$a_0 = 1$$. $$p, q$$ are integers. The symbol $$[\cdot]$$ represents the
floor.  The question is to find the value of $$a_n$$ for any given
$$n$$, $$p$$, and $$q$$.  The constraints are that $$ 0 \leq n \leq 10^{12}
$$ and $$ 1 < p, q \leq 10^9 $$.

### A Simpler Problem: When p Equals q 
Often times, we may find some inspiration by solving a simpler version of the problem.
Let us assume $$p=q$$. The recursion becomes $$a_i = 2a_{[i/p]}$$. Can we solve this one? 
One thing we notice is that, the consecutive $$a_i$$ may have the same value if $$[i/p]$$ are
the same, i.e. $$a_{11} = a_{10} = a_{9} = 2\cdot a_3 $$.  So, instead of
computing $$n$$ values, we can only compute $$n/p$$ values. If $$[n/p]=[m/p]$$,
$$m,n$$ are in the same equivalent class.  But this is not good enough. Let's
see how to compute $$a_{12}$$. $$a_{12}=2\cdot a_{4} = 2 ( 2 a_{1} ) $$. This
is interesting. Although 12 can be divided by 3, but the intermediate index 4
cannot, which results that $$a_{12} = a_{9}$$. It looks like there is a better way to define
the equivalent class. This also means that $$a_{36} = 2 a_{12} = 2 a_{9} = a_{27}$$. This is 
an Ah-ha moment, the (1D) space is actually divided into equivalent classes by
the power of $$p$$.  So, for any given $$n$$, the $$a_n$$ is equal to the
closest $$m$$ such that $$ m = p^x $$, where $$x$$ is some integer. And since the recursion is 
very simple, we can find $$a_m$$ immediately: $$a_m = 2^{ [\log(m)/\log(p)] + 1 } $$ or in code
`2ull << (int)floor( log(n) / log(p) );`. The time complexity is $$O(1)$$.

### A Progressing Algorithm
When $$ p \neq q $$, the space is in 2D. The space is divided into equivalent
classes by the combination of powers of $$p$$ and $$q$$. For example, if
$$p=2$$ and $$q=3$$, any number $$n$$ will have the same value as $$m$$,
where $$m = 2^x 3^y$$, where $$x$$ and $$y$$ are integers and $$m$$ is the
largest number no greater than $$n$$. Let's denote $$M$$ as the set of all numbers like $$m$$ given bases $$p$$ and $$q$$.  It might be possible to solve this
recursion analytically, like the simpler case above, yielding a constant time
algorithm. But I have not figured out how to do so. Alternatively, it is 
feasible to just generate all $$a_k, k\in M$$ using a progressing algorithm. (I call
this algorithm progressing. Please let me know if you know its real name.)
Notice that, $$m\in M$$ is not $$n$$; $$m$$ is the representer of the equivalent
class containing $$n$$. The size of $$M$$, with $$m$$ as the largest element,
should be in the scale of $$O(\log_p(m) \cdot  \log_q(m))$$.

The algorithm goes as the following. Keep an array of pairs of index $$i$$
and its value $$a_i$$. Initialize this array, `v`, by the base cases: $$a_0 =
1$$ and $$a_1 = 2$$. We will also keep two running indices `pidx` and `qidx`,
initialized at 1. Then, we keep generating the next multiplier $$k$$ by
picking the smaller one between `p*v[pidx].first` and `q* v[qidx].first`,
`u`.  Then, calculate its value by looking up the values of `[u/p]` and
`[u/q]` using binary searches. Add the pair `( u, a_u )` into `v`, and
increment the running index (p, or q, or both), from which we pick `u` from.
We keep doing this until $$ u >= n $$.

### Time Complexity
As the maximum number of elements in the vector is 
$$ L \equiv \log_p(m) \log_q(m) $$, where $$m$$ is the largest element in $$M$$ that is no greater than $$n$$.
At each step, we have to perform two binary search taking $$O(\log(k))$$, where
$$k$$ is the size of the vector at that step. Therefore, the total time
complexity is $$ O( \sum_{k=2}^{L} \log(k) )$$, which is $$O( \log( L! ) )$$.
We can use the
[Sterling](https://en.wikipedia.org/wiki/Stirling%27s_approximation) formula
to approximate it as $$ O( L \log L ) $$. For the largest case, 
where $$n=10^{10}$$ and $$p=2$$ and $$q=3$$, $$L\simeq 10^3$$. So, 
this algorithm is efficient enough.

``` cpp
class InfiniteSequence {
  public:
    long calc( long n, int p, int q ) {
      if( n == 0 ) return 1;
      if( n == 1 ) return 2;
      if( p == q ){
        return 2ull << (int)floor( log(n) / log(p) );
      }
      if( p > q ) swap( p,q ); // p < q
      vector< pair< long, long > > v; // n and a[n]
      v.push_back( make_pair( 0, 1) );
      v.push_back( make_pair( 1, 2) );
      int pidx = 1;
      int qidx = 1;
      while( v.back().first < n ){
        long t = v[pidx].first * p ;
        long w = v[qidx].first * q ;
        long u = min( t, w );
        long x = u/p;
        long y = u/q;
        auto itr1 = lower_bound( v.begin(), v.end(), make_pair(x, 0l) );
        auto itr2 = lower_bound( v.begin(), v.end(), make_pair(y, 0l) );
        if( itr1->first > x ) --itr1;
        if( itr2->first > y ) --itr2;
        v.push_back( make_pair( u, itr1->second + itr2->second ) );
        if( t == w ){
          ++ pidx;
          ++ qidx;
        }else if( t < w ){ 
          ++ pidx;
        }else{ // t > w
          ++ qidx;
        }
      }
      if( v.back().first == n ){
        return v.back().second;
      }else{
        return (v.end()-2)->second;
      }
    }
};

``` 


