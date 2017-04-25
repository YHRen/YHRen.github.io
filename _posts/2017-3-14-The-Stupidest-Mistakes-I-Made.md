---
layout: post
title: The Stupidest Miskates I Have Made
published: true
date: 2017-03-14
update: 2017-04-25
tag: [fun]
---

Here is a list of the stupidest mistakes I have made so far. 
So, from time to time, I can look at them and laugh at myself. 😂

`title += "So Far..."`

**NOTE: all codes are mistakes!**

### How to convert a `char` to an `int` in C++
{% highlight cpp %}
char c = '1';
int  a = int(c);
assert( a == 1 ); // NO!
{% endhighlight %}

### How to give a pointer a `new` content in C++
{% highlight cpp %}
int * ptr = nullptr;
void foo( int* ptr ){
  ptr = new int(1);
}
assert( *ptr == 1 ); // NO!
{% endhighlight %}

### How to grow a vector and iterate it at the same time in C++
{% highlight cpp %}
// generate all products of 2^a \times 3^b
vector<int> v = {1};
int p = 2, q = 3, t = 30;
auto pitr = v.begin();
auto qitr = v.begin();
while( t -- ){
  int x = *pitr*p, y = *qitr*q;
  v.push_back( min( x,y ) );
  if( x < y ){
    ++ pitr;
  }else if ( x > y ) {
    ++ qitr;
  }else{
    ++ pitr;
    ++ qitr;
  }
}
assert( v[3] == 4 && v[7] == 12 ); // No!
{% endhighlight %}

### How to use rval-reference in C++ to avoid copying
{% highlight cpp %}
// generate all products of 2^a \times 3^b
struct BitArray{
  uint32_t* ptr; // stores bits
  BitArray( size_t N ){ ptr = new uint31_t[N]; }
  BitArray & operator=(BitArray && rhs){ // rval-ref
    ptr = rhs.ptr; // move the ownership of the allocated chunk to avoid copy
    rhs.ptr = NULL; // so it does not deallocate the chunk
  }
};
/* can you spot a memory leak? */
{% endhighlight %}

### Solutions
If you have not figure it out:

* C++, like C, the conversion is a change of interpretation of the same segment
  of memory. C is using ASCII, a 7-bits table. Every char has 8-bits, or
  2-hexadecimal. For example, char '0' is `30` in hex, which means three 16s.
  So it is 48 in `int`. The right way to convert a char from digit to int is by
  the difference of `c - '0'`.

* This is one is a funny one. The function passes a copy of the pointer, a pointer
  points to whatever memory address the original pointer pointing to. The
  memory allocated within the function is pointed by this temporary pointer. After
  the function ends, this temporary pointer is destroyed, and the original pointer 
  left unchanged. Worst of all, the allocated memory can never be deallocated, classic
  memory leak. The right way is to pass the pointer by reference: `foo( int*& ptr )`;

* When vector is growing, it will reallocate new memory, probably at some other
  place, once its size reaches any power of 2. This will make iterators,
  essentially a pointer, invalid. The right way is either to `reserve` enough
  space at the beginning, `v.reserve(50)`.  If the size is unknown at the
  beginning, probably the best practice is to use indices.  Note that, a
  similar issue also occurs when one delete elements while traversing a binary
  tree. I think C++11 provides a way to solve it as `erase` method returns an
  iterator.  The BGL, boost graph library, also makes vertex (edge) iterators
  invalid if the graph topology changes. Same idea.

* Before assign the `ptr` to the new address, we need to deallocate its memory.
  `if (allocated) delete [] ptr; ptr = rhs.ptr;`
