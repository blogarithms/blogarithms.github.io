---
layout: post
title: "Fenwick Trees!"
excerpt: "A data structure often associated with range queries and invertible functions."
categories: [algorithm]
image:
    feature: 
comments: true
---

Fenwick trees are a pretty cool data structure.

In my experience using them, they're best used to perform range operations over invertible functions. (Find the sum/product of numbers in a given range etc)

I've seen segment trees solve a larger range of problems, but the issue with segtrees is that they usually require more space in terms of heap memory, and also they're relatively more verbose to implement.

The entire motivation I myself had when it came to learning fenwick trees, is that the implementation is often very short and simple.

I'll first explain what's happening, and then highlight my implementation, to make my case.

A fenwick tree relies on bit operations and some preprocessing and allows range querying or point updates in O(logN).

I'll relate it with a segment tree. 

Segment trees are binary trees where each node stores the result of its children. This result could be any operation - sum, product, maximum or minimum.

So in general, a segment tree's node relies on its left child and right child. Easy to picture.

Fenwick trees, are **not** easy to picture.

The simple reason being -- don't picture them as traditional trees. The actual children of every node are fairly non-intuitive at first, so only begin picturing them as trees until you're very familiar with them.

Fenwick trees rely on bitwise operations. What that means is that every node depends on the binary notation of it.

This is done, by instructing each node ```i``` to store the range of values only from ```i``` to ```i - LSOne(i)```

Now that's a big leap in statements and some unfamiliar words too -- I'll explain.

```LSOne(i)``` is the least significant bit of ```i```.

For example, 

```
LSOne(10) = 2 [since 10 in binary is ```1010``` and therefore the least significant bit is 2]

LSOne(9) = 1 [since 9 in binary is ```1001``` and therefore the least significant bit is 1]

LSOne(8) = 0 [since 8 in binary is ```1000``` and therefore the least significant bit is 0]
```

Similarly, LSOne(14) is 2 and LSOne(15) is 1.

Now node ```i``` stores the result of the range operation from index ```i``` to index ```i-LSOne(i)```

So now that we've gotten this, let's see how we can use this in an example:

Let's consider fenwick[6].

fenwick[6] will store the operation of range of [6 to 4), which is just 6 and 5.

Thus if we must calculate the range sum, this is what fenwick[6] stores:

fenwick[6] = A[6] + A[5] + fenwick[4] + fenwick[0];

fenwick[4] = A[4] + A[3] + A[2] + A[1] + A[0]

This is because, in general, fenwick[i] = sum(A[i]...A[i-LSOne(i)]) + fenwick[i-LSOne(i)] + fenwick[i - LSOne(i) - LSOne(i-LSone(i))] + ... keep going down till the index reaches 0.

So, every index in the fenwick tree stores a value corresponding to a certain range of the input array.

Thus, using this cleverly, we can calculate prefix sums in ```O(logN)```. The mathematical reasoning behind WHY its ```O(logN)``` is because the algorithm iterates for as many times as there are set bits in the binary representation of the index, which in the worst case is logarithmic in nature.

Here's my C++ implementation for calculating the prefix sum from index ```0``` to ```i```.

```c++
int query(int idx){
    int ans = 0;
    while(idx > 0){
        ans += fenwick[idx];
        idx -= idx&-idx;
    }
    return ans;
}
```

What this function does, is it calculates sum of the required indexes in the fenwick tree that correspond to ranges in the input array, such that the fenwick tree indexes visited cover the entire range from index 0 to index ```idx``` in the input array.

Similarly, updating the fenwick tree is simple, because here we just need to invert the direction of tree traversal, and propogation.

Therefore, if we were to update index ```idx``` in the input array A by ```val```, here's how the code would look like -

```c++
int update(int idx, int val){
    while(idx<=n){
        fenwick[idx]+=val;
        idx+=idx&-idx;
    }
}
```

And this is the coolest thing about fenwick trees! Their implementation is so small, that if you can identify their use in an algorithmic challenge, implementing it would be a breeze.

Another benefit of fenwick trees, is that their pretty lightweight in terms of space complexity. Segment Trees require a space complexity of ```O(4*N)```, while fenwick trees only needs ```O(N)```, and so it's also useful when being used as a hash map.

Consequently, it can be used efficiently in solving otherwise pesky problems, such as finding the count of inversions in an array (the number of pairs of elements in an array where A[i]>A[j] and i<j) in linearithmic time.

Cheers!