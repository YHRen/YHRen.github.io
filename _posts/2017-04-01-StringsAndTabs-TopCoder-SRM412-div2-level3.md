---
layout: post
title: StringsAndTabs topcoder SRM412 div2 level3
published: true
date: 2017-04-01
tag: [topcoder, div2, level3, greedy]
---

### Introduction
If you have not tried to solve this
[problem](https://community.topcoder.com/stat?c=problem_statement&pm=8478)
by yourself, please do so. Otherwise, this post will make little sense to you.

This problem has a very interesting background: translate a tablature (tab)
from one string instrument to another, i.e. guitar to ukulele
or balalaika and vise versa. Knowing the tab system definitely helps, but even if one 
has no background on tab, the explanation is very clear. 🎸 

The solution is straightforward, but to write a bug-free code in a 
short period is challenging. 

### A Greedy Approach
For each time step, get all notes from the input
tab. Raise the pitch by $$d$$, which can be negative. Then, look for strings on the output 
instrument to play all notes for this time step. There are some rules to deal with degeneracy.
If a note can be played on more than one strings, play it on the one with highest base
pitch; and if two strings have the same base pitch, play it on the one with highest 
index (bottom-most). So, it might be a good idea to sort the output instrument strings
int such order, and we can do binary search to look for such string. At the end, we can
sort them again according to their indices. Therefore, it is a good idea to pack the base
pitch, index and note sequence into a `class`, called `Str` in my code.

Another difficulty is to manage an infeasible cord correctly.

* a note is too high or too low for the output instrument;
* a note cannot be played anymore because all possible strings have been occupied.

I did this by a `bool` flag, and a vector to track which strings have been
played.  If all notes can be played, attach a `'-'` to all unvisited strings.
Otherwise, pop the last notes from the visited strings and attach `'x'` to all strings.

The default comparator of `lower_bound()` is `less` and it assumes the vector
is in ascending order. It will return the first element which does not compare
*less* than the query value. However, in our case, the strings are in
descending order with respect to the base pitch. We can still use `lower_bound()`by
passing a (customized) `greater` comparator. It will return the first element
which does not compare *greater* than the query value. Here is a little demo.

``` cpp
int main( int argc, char * argv[] ){
  vector<int> v= { 1, 2, 3, 3, 3, 5, 5, 7};
  auto itr = lower_bound(v.begin(),v.end(),3);
  cout << itr-v.begin() << '\n'; // return 2, the index of the first '3'
  sort(v.begin(),v.end(), std::greater<int>()); // {7, 5, 5, 3, 3, ...}
  for( auto x : v ) cout << x << ' '; cout << '\n';
  itr = lower_bound(v.begin(),v.end(),4, greater<int>()); 
  cout << itr-v.begin() << '\n'; // return 3, the first '3'. as '3'<='4'
  itr = lower_bound(v.begin(),v.end(),3, greater<int>());
  cout << itr-v.begin() << '\n'; // return 3, the first '3'. as '3'<='4'
  return 0;
}
```
### Time Complexity
If there are $$N$$ time steps, $$M$$ strings of input instrument, and $$K$$
strings of output instrument, the running time of worst case is $$O(N\ M\log
K)$$.

``` cpp
class StringsAndTabs {
  private:
    struct Str {
      int idx;
      int pitch;
      string notes;
    };
    vector<Str> v;

    void assign_all( const char c ){
      for( auto & x : v ){
        x.notes += c;
      }
    }

    void translate( vector<int> & notes ){
      if( notes.size() == 0 ){
        assign_all( '-' );
        return;
      }

      vector<int8_t> is_visited( v.size(), false );
      bool can_assign = true;
      for( auto x : notes ){
        Str tmp; tmp.pitch = x;
        auto itr = lower_bound( all(v), tmp, [](
            const Str & lhs, const Str & rhs){
            return lhs.pitch > rhs.pitch;
            });
        // too high or too low
        if ( (itr == v.begin() && itr->pitch + 35 < x) || // too high
            ( itr == v.end() && (itr-1)->pitch > x )  ){ // too low 
          can_assign = false;
        }else{
          if( itr == v.end() || itr->pitch > x )  -- itr;
          while( is_visited[ itr - v.begin() ] ){ // find next string to accomodate
            ++ itr;
            if( itr == v.end() ){
              can_assign = false;
              break; // cannot assign anymore
            }
          }

          if( can_assign ){
            int y = x - itr->pitch;
            if( y <= 9 ){
              itr->notes += to_string( y );
              is_visited[itr-v.begin()] = true;
            }else if( y <= 35 ){
              itr->notes += 'A'+(y-10);
              is_visited[itr-v.begin()] = true;
            }else{ // y is too large
              can_assign = false;
            }
          }
        }
        if( !can_assign ) break;
      }

      if( !can_assign ){
        for( int i = 0 ; i < is_visited.size(); ++i ) {
          if( is_visited[i] ){
            v[i].notes.pop_back();
          }
        }
        assign_all( 'x' );
      }else{
        for( int i = 0 ; i < is_visited.size(); ++i ) {
          if( ! is_visited[i] ){
            v[i].notes.push_back( '-' );
          }
        }
      }

    }
  public: 

    vector<string> transformTab( 
        vector<string> & tab, 
        vector<int> stringsA, 
        vector<int> stringsB, 
        int d ){
      
      // initialize strings of output instrument
      int n = stringsB.size();
      this->v.resize(n);
      for( int i = 0; i < stringsB.size(); ++i ){
        v[i].idx = i;
        v[i].pitch = stringsB[i];
        v[i].notes = "";
      }
      // sort all strings based on pitch and then index
      sort( v.begin(), v.end(), [](const Str & lhs,
            const Str & rhs ){
          if( lhs.pitch > rhs.pitch ) return true;
          else if ( lhs.pitch == rhs.pitch && lhs.idx > rhs.idx )
          return true;
          else return false;
          } );

      const int M = tab[0].size();
      const int K = tab.size();
      for( int i = 0; i < M; ++i ){ // for each time step
        vector<int> notes;
        notes.reserve(K);
        for( int j = 0; j < K; ++j ){
          char c = tab[j][i];
          if( c != '-' ){
            if( isdigit(c) ){
              notes.push_back( c - '0' + d + stringsA[j] );
            }else{ // c is A-Z
              notes.push_back( c - 'A' + 10 + d + stringsA[j] );
            }
          }
        }
        sort( all(notes), std::greater<int>());
        translate( notes );
      }

      // sort by index, to output
      sort( all( v ), []( const Str & lhs,
            const Str & rhs ){
          return lhs.idx < rhs.idx ;
          } );

      vector<string> rst( v.size() );

      for( int i = 0; i < rst.size(); ++i ){
        rst[i] = move( v[i].notes );
      }
      return rst;
    }

};
/* the main() for testing.
int main( int argc, char * argv[] ){
  std::ios::sync_with_stdio(false);
  int n,m,d; cin >> n >> m >> d;
  vector<string> tab(n);
  vector<int> stringsA(n);
  vector<int> stringsB(m);
  input( tab );
  input( stringsA );
  input( stringsB );
  StringsAndTabs s;
  auto rst = s.transformTab( tab, stringsA, stringsB, d );
  for( auto x : rst ) cout << x << '\n'; 
  return 0;
}
*/
```
