---
layout: post
title: "C++ Standard Template Library Hacks I like"
excerpt: "Leveraging C++'s STL to write code faster."
categories: [development]
image:
    feature: 
comments: true
---

C++'s STL is a boon. Not only is C++ extremely fast, in terms of execution, for programming contests -- but the addition of STL makes it an extremely strong contender for my "favorite programming contest language".

For this article, I'll assume the reader has a basic familiarity of STL and knows the following data types of STL: vector, map, set, priority_queue, stack, queue, multiset and multimap.

Thus, I won't go into their built-in functions such as find(), count(), push(), pop(), push_back() etc.

After all, those are available everywhere.

This article will focus on what STL commands I find really cool that I don't find online often -- so hopefully by the end of this article, you'll have some cool ideas on how to make your next code shorter.

In ascending order of "coolness" based on my opinion, here are some of my favorite STL shortcuts.

-------

### 1. __gcd(a, b)

The method to find the gcd of two numbers is `__gcd(a, b)`. (Note: There are TWO underscores, not ONE before the function name)

This method is defined under `#include<bits/stdc++.h>` and returns the greatest common divisor of integers or longs `a` and `b`.

This is my go-to in majority of number theory problems involving GCDs or LCMs.

--------

### 2. __builtin_popcount(a)

This method returns the number of ones present in the binary notation of the number.

i.e. `__builtin_popcount(7)` = 3, `__builtin_popcount(5)` = 2, `__builtin_popcount(4)` = 1

-------

### 3. __builtin_ctz(a)

This method returns the number of trailing zeros of a number `a`. Consequently, I've also found this convenient in cases where I'm trying to find the least significant bit of a number.

eg. `__builtin_ctz(16)` = 4, `__builtin_ctz(15)` = 0

In fact, this could replace the clever bitwise operation `a&-a` which directly returns the least significant one of a number.

If the above expression is new to you, consider reading [this article from Topcoder on bitwise operations](https://www.topcoder.com/community/competitive-programming/tutorials/a-bit-of-fun-fun-with-bits/).

-------

### 4. auto

This is one of my most overused feature of STL, `auto` for C++ is similar to the enhanced for loop of Java, except here we don't need to even define the datatype of the iterator as it AUTOmatically is defined.

Here's a sample.

```c++

vector<int> A = {2, 4, 3, 5};

// boring way
for(int i=0; i<A.size(); i++) cout << A[i] << " ";

// fun way
for(auto i: A) cout << i << " ";

```

And this is incredibly convenient if I had a set, or a map, where I'd otherwise have to define an iterator and use a corresponding exit condition.

Check it -

```c++

set<int> s = {8, 2, 3, 5, 9};

// boring way
for(set<int>::iterator i = s.begin(); i!=s.end(); i++) cout << *i << " ";

// fun way 
for(auto i:s) cout << i << " ";

```

-------------

### 5. swap(a, b)

Literally does what it says. 

`swap(a, b)` swaps the value of a with b and vice versa. Convenient.

--------------

### 6. lower_bound(arr.begin(), arr.end(), val)

This method implements binary search on an array. It assumes the array is sorted, and returns the position at which `val` must be inserted for the array to remain sorted.

Thus, calling this method on an array {10, 20, 30, 40, 50} with `val = 15` would return `1`.

The difference between lower_bound and upper_bound is evident when val is also an element present in the array, in which case lower_bound returns an index less than upper_bound.

----------------

### 7. accumulate(arr.begin(), arr.end(), init)

This method is really convenient. 

It returns the sum of all elements + init.

Here's how it looks:

```c++

vector<int> v{1, 2, 3, 4, 5, 6, 7, 8, 9, 10};

int sum = std::accumulate(v.begin(), v.end(), 0);

```

`sum` now stores 55 in it.

This method also has a few more parameters that you can modify, such as the mathematical function to apply between an elements and the result of all preceding elements.

The default operation is addition but it can be overridden to support GCD or Multiplication by adding a 4th parameter similar to adding a 'cmp' parameter in STL's sort method.

----------------------

### 8. partial_sum(arr.begin(), arr.end())

Another very useful method, it overwrites an array with its own prefix sum.

For example,

```c++

vector<int> arr(10, 2); // creates a vector of size 10, populated with values of 2
partial_sum(arr.begin(), arr.end());
for(auto i:arr)cout<<i<<" ";

```

This outputs `2 4 6 8 10 12 14 16 18 20` and so now range sum queries are O(1), with prefix sum arrays implemented in just a single line.

--------------

### 9. adjacent_difference(arr.begin(), arr.end(), res.begin())

Generates the difference array of `arr` and stores it in `res`.

If you aren't familiar with difference arrays, I would point you to [my article on difference arrays](/articles/2018-11/difference-arrays) which allow you to perform range updates on an array in O(1). 

Here's how that looks:

```c++

vector<int> a = {1, 2, 4, 7, 12, 19};
vector<int> b(6);

adjacent_difference(a.begin(), a.end(), b.begin());

for(auto i:b)cout<<i<<" ";

```

This yields an output of `1 1 2 3 5 7` which if you'll notice is the difference array of `a`.

--------------------

### 10. iota(arr.begin(), arr.end(), val)

The final method on my list, is `iota` which populates an array/vector in increasing values starting from `val`.

Here's how it looks in code:

```c++

vector<int> a(10);
iota(a.begin(), a.end(), 2);

for(auto i:a)cout<<i<<" ";

```

This yields an output of `2 3 4 5 6 7 8 9 10 11`. This is highly convenient when using hash maps, or leveraging them as a component of your main algorithm, such as in the case of disjoint-set-union, or union-find algorithms.

If you aren't familiar with DSU, consider reading [my technical blog post explaining it over here](/articles/2017-12/diving-into-disjoint-sets) and then try solving a [problem from Topcoder over here](/articles/2018-09/topcoder-weekly-tco-disjoint-set-union) -- and worry not, that link also contains my explanation on how to solve that problem. :)

--------------------------

Well! I hope this added some value to you! Feel free to let me know your comments. :)

For more of my work, follow me on:

LinkedIn: [https://www.linkedin.com/in/adityaramesh1998/](https://www.linkedin.com/in/adityaramesh1998/)

GitHub: [https://github.com/RameshAditya/](https://github.com/RameshAditya/)

Twitter: [https://twitter.com/adityaramesh98](https://twitter.com/adityaramesh98)

Cheers!