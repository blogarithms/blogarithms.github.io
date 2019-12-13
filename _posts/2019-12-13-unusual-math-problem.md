---
layout: post
title: "An unusual math problem - Algo Spotlight of the Week"
excerpt: "Taking a look at a rather nice math challenge."
categories: [algorithm]
image:
    feature: 
comments: true
---

This was one of the problem statements that I encountered recently in a contest I competed in, for fun.

The crux of the problem statement was -

Given a number N (1 <= N <= 1000), find the number of sets (i, j, k, x, y, z) where the following constraints hold true: 

```
(i+2j+k) % (x+y+2z) = 0
``` 

and 

```
1 <= x,y,z,i,j,k <= N
```


-----------------------------------------------------------------------------

If you want to think over this challenge yourself, now would be the time. My solution of O(N^2) netted me all 100 points, so any solution atleast as good would suffice.

--------------------------------------------------------------------------------

So let's jump in! 

First, the brute force - 

The laughable solution of O(N^6) is painfully obvious - iterate over every sextuplet and verify the constraints.

An insight to make is that we want the (x+y+2z) to be a factor of (i+2j+k), so alternatively we could just process the factors of our current triplet - but that's still not fast enough.

Next observation is that, well, (i+2j+k) and (x+y+2z) have the same set of coefficients and since there's no mapping constraints between these terms, they both belong to a general set of elements whose form is A+B+2C.

Thus we could instead just generate this global set where each element is representable as A+B+2C where A, B, C lie between 1 and N and process its factors each.

But again, this is still O(N^(3.5)) at best.

Next observation - 

Let's define F(X) as the number of ways to generate X where X = A+B+2C and 1 <= A, B, C <= N.

Now the question reduces to computing F(X) * F(Y) for every X from 4 to 4 * N, and every Y which is a factor of X. 

The reason we vary X from 4 to 4 * N is because the maximum value X can take is 4 * N which is when A = B = C = N.

Computing F(X) * F(Y) over all those values of X and Y is O(N * sqrt(N)) so that's feasible.

But how do we compute F(i) in general?

Let's fix C to some constant value.

Then for a fixed, current value of i, the question then becomes - 

What are the number of ways I can generate pairs (A, B) where A + B + 2C = i?

In other words, find number of pairs (A, B) where A+B = i - 2C and (1 <= A, B <= N)

This is very solvable in O(1).

Take the example of A+B = 10 where (1 <= A, B <= 6).

Here the answer is (4,6), (5, 5), (6, 4) - basically the range of values A can take would be from 

`max(min(A), 10-max(B))` to `min(max(A), 10 - min(B))`, which is (10-6) to 6, which is 4 to 6.  

Now that we can compute F(i) in O(1) for a given i and C, we iterate over all N values of C, and all 4N values of i.

And with that, now that we have F(i) for every i, we can compute the answer as discussed above. :)

And that solves this in O(N^2) in total. 

This was quite a fun math problem that I was surprised was in the contest since the previous question was a simple one on sorting.

Oh well. 

----------------------------------------------------------------------------------------

For more of my work, follow me on:

LinkedIn: <span style="color:blue">[https://www.linkedin.com/in/adityaramesh1998/](https://www.linkedin.com/in/adityaramesh1998/)</span>

GitHub: <span style="color:blue">[https://github.com/RameshAditya/](https://github.com/RameshAditya/)</span>

Twitter: <span style="color:blue">[https://twitter.com/adityaramesh98](https://twitter.com/adityaramesh98)</span>  