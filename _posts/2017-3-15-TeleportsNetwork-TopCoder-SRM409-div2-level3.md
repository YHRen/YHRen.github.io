---
layout: post
title: TeleportsNetwork topcoder SRM409 div2 level3
published: true
date: 2017-03-15
tag: [topcoder, div2, level3, graph algorithm]
---

### Introduction
If you have not tried to solve this
[problem](https://community.topcoder.com/stat?c=problem_statement&pm=9830) by
yourself, please do so. Otherwise, this post will make little sense to you.


Solution to this problem is straightforward, which can
be broke down into two parts: 
 1. construct the graph; 
 1. find the min-max inconvenience.

### Part-I: Graph Construction
Let $$G(V,E)$$ be the graph, and we know the
[order](http://mathworld.wolfram.com/GraphOrder.html) of $$G$$ is $$\vert
V\vert = n$$.  Because each city, except the capital, is going to build one
road, the total number of edges is $$\vert E\vert = m = n-1$$.  The rule of
building a road from a city $$i$$ can be restated as "find the closest city
$$j$$, $${\arg\min}_{j\in V,\ j\neq i} d(i,j)$$, such that $$d(0,j) <
d(0,i)$$", where $$d(\cdot)$$ is the Euclidean distance between two cities. If
the situation is
[degenerate](https://en.wikipedia.org/wiki/Degenerate_energy_levels), use $$x$$
and $$y$$ coordinates to resolve it. This is done in the function of
`build_road()`. 

There are many ways to compute the distance and build roads. 
As the graph is small, $$ n \leq 50 $$, we can just compute the pair-wise
distance once, sort each row, and find the first city satisfying the condition.
Alternatively, we can compute the distance to the capital from all the cities,
and get the sub-vector per city. Then, for city $$i$$, get all the cities that
is closer to the capital. Then, among them, find the one that is closest to
$$i$$. But we need to handle the degeneracy carefully. 


### Part-II: Find Min-Max-Inconvenience
This part is very interesting. I think it is at least as difficult as the
well-known NP-complete [maximum coverage
problem](https://en.wikipedia.org/wiki/Maximum_coverage_problem). 
The good news is that the graph is small,
at most $$n=50$$ nodes; and the number of teleports is at most $$p=4$$.
The total number of combinations is $$ {n \choose p} = {50 \choose 4 } \simeq
2\times 10^4$$. For each configuration, finding the max inconvenience in the
graph is simply a BFS with time complexity of $$O(N+M)$$. As $$ m = n-1 < 50
$$, in total, the number of operations is in the magnitude of $$10^6$$, which
is manageable.

I use a recursive method in hope to save some computation time as the distance
vector can be reused for planting the next port.
Alternatively, we can compute the inconvenience vector of placing a teleport at each 
city. The inconvenience vector of placing more than one teleports is a
reduction of element-wise minimization. (By adding another teleport, the
inconvenience is only to decrease.) Then, we can iterate through all combinations
with size $$p$$ teleports via `std::next_permutation()` on a binary vector with
size $$n$$ and $$p$$ bits set. Then, for each configuration, measure its
max inconvenience by reduction of pre-computed inconvenience vectors. 

{% highlight cpp linenos %}
/******* Actual Code Starts Here *********/
// Problem: https://community.topcoder.com/stat?c=problem_statement&pm=9830
//
// Signature:
// Class: TeleportsNetwork
// Method:  distribute
// Parameters:  int, int[], int[]
// Returns: int
// Method signature:  int distribute(int teleportCount, int[] citiesX, int[] citiesY)
// 
// Constrains:
// -  citiesX will contain between 1 and 50 elements, inclusive.
// -  teleportCount will be between 0 and 4, inclusive and less than the number of elements in citiesX.
// -  Each element of citiesX will be between 0 and 1000000, inclusive.
// -  citiesY will contain the same number of elements as citiesX.
// -  Each element of citiesY will be between 0 and 1000000, inclusive.
// -  No two cities will occupy the same point.

class TeleportsNetwork {
  int n;
  int k;
  vector<vector<double>> dist_mtx;
  vector<vector<int>> graph;

  inline double get_dist(int x1, int y1, int x2, int y2) {
    double dx = x1 - x2;
    double dy = y1 - y2;
    return sqrt(dx * dx + dy * dy);
  }

  void comp_dist(const vector<int>& x, const vector<int>& y) {
    dist_mtx.resize(n);
    for( auto & v : dist_mtx ) v.assign(n,0);
    forall(i, 0, n) {
      forall(j, i + 1, n) {
        dist_mtx[i][j] = dist_mtx[j][i] = get_dist(x[i], y[i], x[j], y[j]);
      }
    }
  }

  void build_road(const vector<int>& x, const vector<int>& y) {
    forall(i, 1, n) {
      vector<pair<double, int>> tmp(n);
      forall(j, 0, n) { tmp[j] = mp(dist_mtx[i][j], j); }
      sort(all(tmp),
           [&](  // sort based on dist to city i
               const pair<double, int>& lhs, const pair<double, int>& rhs) {
             return mp(lhs.first, mp(x[lhs.second], y[lhs.second])) <
                    mp(rhs.first, mp(x[rhs.second], y[rhs.second]));
           });
      forall(j, 0, n) {
        if (dist_mtx[tmp[j].second] < dist_mtx[i]) {
          int u = tmp[j].second;
          graph[i].pb(u);
          graph[u].pb(i);
          break;
        }
      }
    }
  }

  void update( int idx, vector<int> & v ){
    vector<int8_t> visited(n, false);
    v[idx] = 0;
    visited[idx] = true;
    queue<int> Q;
    Q.push( idx );
    while( ! Q.empty() ){
      int x = Q.front();
      Q.pop();
      for( int u : graph[x] ){
        if( ! visited[u] ){
          visited[u] = true;
          if( v[u] > v[x]+1 ){
            v[u] = v[x]+1;
            Q.push(u);
          }
        }
      }
    }
  }

    
  int place_port(int idx, int r,
      vector<int> & v ) {
    if( idx == n && r != 0 ){
      return INF;
    }
    if( r == 0 ){
      return *max_element( all(v) );
    }
    int rst = INF;
    for(int i = idx; i < n; ++i ){
      auto u = v;
      update(i, u); //place port at idx, update v
      int p = place_port( i, r-1, u ); 
      rst = min( rst, p);
    }
    return rst;
  }

 public:
  int distribute(int teleportCount, 
      const vector<int>& x, const vector<int>& y) {
    this->n = x.size();
    this->k = teleportCount;
    this->graph.resize(n);
    comp_dist(x,y);
    build_road(x,y);
    vector<int> v(n,INF);
    update(0,v);
    return place_port( 1, this->k, v);
  }
};
{% endhighlight %}


