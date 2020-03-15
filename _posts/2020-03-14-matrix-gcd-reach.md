---
layout: post
title: "Matrices and GCD"
excerpt: "A math thing that pops up occasionally"
categories: [algorithm]
image:
    feature: 
comments: true
---

I encountered this challenge recently in a coding contest online.

The crux of the problem statement was that you start on a matrix at cell (1, 1). The matrix is as large as (10^18, 10^18) with each cell correspondingly labelled as some (x, y).

Your task is to answer Q queries, where each query is of the form 'X Y', and you need to print 'Yes' if you can reach (X, Y) from (1, 1), else print 'No'.

Your movement is constrained by the following rules:

- If you're at (x, y), you can bidirectionally move to (2x, y) or (x, 2y).
- If you're at (x, y), you can bidirectionally move to (x-y, y) or (x, y-x)

Solve the problem.

An example of a state that can't be reached, would be (3, 6), or (7, 21). An example of reachable states would be (2, 10) or (5, 1).

--------------------------

If you'd like, take a few minutes to think over this - scroll down for my thought process behind solving it.

--------------------------

Okay, so let's dive in.

Since the cells extend till 10^18, no DFS or BFS solution is of use here. Similarly, we cannot afford to store the actual matrix in memory. So lets make some observations.

First observation - 

the movement criteria mentioned in the question is an obfuscation. The simplified movement criteria are, -

- if you're at (x, y), you can reach any of (x+x, y), (x, y+y), (x+y, y), (x, x+y).

Reversing this, to reach (x,y) we must have been at (x/2, y) or (x, y/2) or (y-x, x) or (x, x-y).

In general, when I see any transition mapping (x, y) to (y-x, y) or (x, x-y), ALONG with (1,1) involved, my mind gravitates to the euclidean theorem for computing GCDs.

But we need to eliminate the second kind of transition before leveraging the GCD idea (which we still haven't fully explored yet, either).

The second kind of transition is (x, y) -> (x/2, y) or (x, y/2).

And since this is completely independent of the other coordinate, we can handle this independently and just repeatedly divide each term by 2 until we reach their final state (which I'll call their 'modified' state).

Second observation -

The state (1, k) or (k, 1) is always reachable. This is because we can always keep adding 1 to the larger coordinate while the smaller coordinate remains unchanged.

With this second observation, we know that if at any point we can reduce our input (X, Y) to the form (1, K), then we can print 'Yes'.

But how do we verify this?

Easily. (1, K) is the second-last state we reach if the GCD is 1. The next state would be (0, 1) when using euclidean's recursive GCD computation algorithm.

So that's it! If the GCD of the coordinates in the modified state is 1, then we can print Yes, else we print No.

```python
from math import gcd
for t in range(int(input())):
    x, y = [int(i) for i in input().split()]
    
    while x%2 == 0: 
    	x = x//2
    while y%2 == 0: 
    	y = y//2 
    
    if gcd(x, y) == 1:
        print("Yes")
    else:
        print("No")

```

And here's my as-code-golfy-as-possible imlementation - 

```python
# 114 chars (can't do better, comment your code if you can down below)
import math
c=int
z=input
for t in range(c(z())):g=math.gcd(*map(c,z().split()));print(["Yes","No"][g&(g-1)>0])

```

