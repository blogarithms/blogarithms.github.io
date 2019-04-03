---
layout: post
title: "Difference Arrays And How They Can Make A Difference"
excerpt: "Perform Invertible Range Updates in O(1) (also, sorry for that pun in the title)"
categories: [algorithm]
image:
    feature:
comments: true
---

Difference arrays are EXTREMELY simple to implement, and very powerful when leveraged correctly.


##### They enable you to perform a range update in O(1) -- but the drawback is that a range query requires O(N) time.

So the best possible use of this data structure is when you have very few range queries and many range updates.

In addition, the range update also must be invertible. You'll soon see why this is necessary.

Now let's get to the definition of a difference array.

As the name suggests, it is **the difference of adjacent elements in an array**

That's it. 

It's the inverse of a prefix sum array, whose recurrence relation is: 

```python
for i in range(len(A)):
    if i==0:
        pref[i] = A[i]
    else:
        pref[i] = pref[i-1] + A[i]
```


Thus, in a difference array, the recurrence relation is: 
```python
for i in range(len(A)):
    if i==0:
        diff[i] = A[i]
    else:
        diff[i] = A[i] - A[i-1]

```



#### Important Line Alert &darr;
Now that we have our difference array, we perform our updates (I'll explain how next) on this array and then take the prefix sum over this array to obtain our final new array, which will be our final result.
#### End of Important Line


So that's the big picture but there's still one missing piece to this puzzle. 
---
##### How do we perform updates on the difference array?

Easy. Let's see how we'd do this if my invertible range update is to add an integer X to all elements in A between index L and index R.

Now the **simple brute-force approach would be** -

```python

A = [int(i) for i in input().split()]
L, R, X = [int(i) for i in input().split()]

for i in range(L, R+1):
    A[i] += X

```

And this is perfectly fine. But if I had to update a range of indexes repeatedly, say some Q number of times, then the brute-force would have a complexity of **O(Q * N)**

Here's how that code would look -
```python

# Read in Array
A = [int(i) for i in input().split()]

# Read in number of queries
Q = int(input())

for query in range(Q):
    L, R, X = [int(i) for i in input().split()]
    for i in range(L, R+1):
        A[i]+=X

```

This has a complexity of O(Q*N).

Let's look at the difference array code for this: (and don't worry, I'll explain what I've done after the code)
```python

# Read in Array
A = [int(i) for i in input().split()]
diff = [0]*len(A)

# Read in number of queries
Q = int(input())

# Generate Difference Array
for i in range(len(A)):
    if i==0:
        diff[i] = A[i]
    else:
        diff[i] = A[i]-A[i-1]

# Iterate over queries
for query in range(Q):
    L, R, X = [int(i) for i in input().split()]

    # Here's where the cool bit happens
    diff[L] += X
    diff[R+1] -= X

# And updating the original array now
for i in range(len(A)):
    if i==0:
        A[i] = diff[i]
    else:
        A[i] = diff[i] + A[i-1]


```


And ```A``` now has all updates performed on it.

---

### How difference arrays work

The premise is very simple.

When you add ```X``` to the ```L```th index of the difference array, when you then take the prefix sum, that ```X``` is carried over onto every subsequent index after ```L```.

That's also why you decrement ```X``` at the ```R+1```th index, as you want the update to end at the ```R```th index (inclusive).

---

Now let's take a look at this data structure with an example algorithmic challenge.

```
Aditya is standing in a room with (N) bulbs in a row. Initially all bulbs are on.

He decides to play a game.

For (Q) number of times, he will toggle all switches between two chosen bulbs. 

He wants to determine how many bulbs will be switched on at the end.

Let L and R depict the two arrays denoting the left and right bounds of bulbs he toggles.

That is, L[i] and R[i] (L[i] <= R[i] and 1<=i<=Q) denote the two chosen bulbs during the (i)th time, in which he'll toggle all bulbs between these two bulbs.

For example ->

N = 5
Q = 3
L = [1, 2, 3]
R = [5, 3, 5]

Meaning, Aditya first toggles all bulbs from the 1st to the 5th.
Then he toggles all bulbs from the 2nd to 3rd.
Then 4th to 5th.

This means, after query 1 -> all bulbs are off.
            after query 2 -> [off, on, on, off, off]
            after query 3 -> [off, on, off, on, on]

Thus, the answer for the given N, Q, L, R is [off, on, off, on, on], in other words - [0, 1, 0, 1, 1]

```

Again, here too, the idea stays the same. The only challenge is to identify an invertible operation.

Now let's think logically. 

Flipping a bulb implies toggling its state.

- If bulb ON, make bulb OFF.
- If bulb OFF, make bulb ON.

The simplest operation I can think of here, is the bitwise XOR operation.

And that's the hint I'll give you, the reader, and ask you to solve it!

Cheers!

~ Aditya Ramesh


P. S. More hints: take initial ```A = [0, 0, 0, 0 ... ]``` and on the difference array, perform ```diff[L] ^= 1``` and ```diff[R+1] ^= 1```
