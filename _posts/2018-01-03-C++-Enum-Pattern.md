---
layout: post
title: C++ Enum Pattern 
published: true
date: 2018-01-03
tag: [object-oriented, c++]
---

### Introduction
Very often we want to define a set of items to choose from, for example, a set
of colors.  In C++, we usually can declare `enum class Color { Red, Green, Blue
}`. However, besides using it in a `switch` statement, we can hardly use it for 
anything else. For example, if we want to print `"Red"` for `Color::Red`, we have to
write another function using `switch` statement, *somewhere else*.  When we
want to add a new color, we have to change every places we are using `switch`.
This is obviously an anti-pattern. Sometime, we also want to find how many colors in total, 
and maybe even iterating through them. None of these are supported by C++
`enum`.  In this blog, I want to introduce the **Enum Pattern** in C++, which
supports `switch` and encapsulated member functions, similar to the Enum Class
in Java. The idea came from this stackoverflow
[post](https://stackoverflow.com/questions/1965249/how-to-write-a-java-enum-like-class-with-multiple-data-fields-in-c).

### The Code
```
class GuitarBrand { // Enum Class Pattern
 public:
  static const GuitarBrand FENDER;
  static const GuitarBrand GIBSON;
  static const GuitarBrand PRS;
  // If we want to add another GuitarBrand Ibanez:
  static const GuitarBrand IBANEZ;

 private:
  static int sz; 
  int index;     
  string name;
  
  // constructor are private 
  GuitarBrand() {}
  GuitarBrand(string str) : name(str) { index = sz++; }

  // if we want to support iterating:
  // vector<GuitarBrand*> vec;
  // GuitarBrand(string str) : name(str) { 
  //  vec.push_back(this); 
  //  index = sz++; 
  // }

 public:
  string get_name(void) const { return this->name; }
  int get_enum_size(void) const { return this->sz; }
  const operator int(void) const { return this->index; } 
};
int GuitarBrand::sz = 0;
const GuitarBrand GuitarBrand::FENDER("Fender");
const GuitarBrand GuitarBrand::GIBSON("Gibson");
const GuitarBrand GuitarBrand::PRS("PRS");
// We also add initialization here for Ibanez:
const GuitarBrand GuitarBrand::IBANEZ("Ibanez");
```

As we can see, the `GuitarBrand` class is self-sustaining.
All member functions and related fields are enclosed within the class.
If we want to add another `GuitarBrand`, i.e. Ibanez, all we need to do is to create
another `static const GuitarBrand` instance, and initialize it.
The `static int sz` tracks the number of different `GuitarBrand` instances (types), and
also assign a proper `index` for each type of guitar brand.
If we like to provide the functionality of iteration of all guitar brands, we
could also add a `vector<GuitarBrand*>` as private member and `push_back(this)`
for each guitar brand construction. The `int` conversion function enable us to use 
`switch` statement on `GuitarBrand` objects.

