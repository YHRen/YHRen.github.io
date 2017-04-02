---
layout: post
title: HoleCakeCuts topcoder SRM411 div2 level3
published: true
date: 2017-03-18
tag: [topcoder, div2, level3, logic]
---

### Introduction
If you have not tried to solve this
[problem](https://community.topcoder.com/stat?c=problem_statement&pm=9752&rd=12183)
by yourself, please do so. Otherwise, this post will make little sense to you.

Given a square cake of size $$L$$ with a square hole of size $$\ell < L $$, and
a set of vertical cuts, $$vcut$$, and horizontal cuts, $$hcut$$, how many
pieces of cake in the end.

### A Solution using Logic 
Let $$n= \vert vcut\vert$$ and $$m=\vert hcut\vert$$, if the cake has no hole, the total number
of pieces is $$ s\equiv (m+1)\cdot (n+1)$$. Now, let us see how the hole affects the result. 

- If all the cuts does not intersect with the hole, the hole has no effect, `return s;`

- If among $$m$$ vertical cuts, there are $$m_{in}\leq m$$ cuts within the
  hole, including ones overlapping with the boundary of the hole, and assume
  $$n_{in}==0$$, we can treat the hole as a single horizontal cut, which doubles the
  pieces within the vertical range of the hole: `return s+ vin - 1;`

- If $$m_{in} > 0$$ and $$n_{in} > 0 $$, the hole nullifies all pieces
  within its boundary, `return s + (vin - 1)*(hin - 1);`

``` cpp
class HoleCakeCuts {
  public:
    int cutTheCake( const int cakeLength, const int holeLength, const vector<int> & hcuts, 
        const vector<int> & vcuts ){
      int n = hcuts.size();
      int m = vcuts.size();
      int vin = 0, hin = 0; // num of cuts within the hole
      for( auto x : hcuts ){
        if( abs(x) <= holeLength ) ++ hin ;
        if( abs(x) >= cakeLength ) -- n ;
      }
      for( auto x : vcuts ){
        if( abs(x) <= holeLength ) ++ vin ;
        if( abs(x) >= cakeLength ) -- m ;
      }
      int s = (n+1)*(m+1);
      if( vin == 0 && hin == 0 ){
        return s;
      }else if( vin != 0 && hin == 0 ){
        return s + vin - 1;
      }else if( vin == 0 && hin != 0 ){
        return s + hin - 1;
      }else{    // ( vin != 0 && hin != 0 )
        return s - (vin-1)*(hin-1);
      }
    }
};
```
