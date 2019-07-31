---
layout: post
title: "Simplifying Square Root Decomposition"
excerpt: "Solving range queries, without a segment tree."
image:
  feature: 
categories: [algorithm]
comments: true
---

I first heard of square root decomposition sometime around my sophomore year of college.

What was funny though was that I had encountered and learnt segment trees and lazy propagation before I stumbled upon square root decomposition.

So when I found out what square root decomposition could do, I was quick to dismiss its use since at the time I deemed segment trees as superior and capable of replicating all functions that square root decomposition could do.

The motivation behind this blog post is that recent events showed me the value of this algorithm.

The key selling point of this algorithm, is - 

**Range query and range updates in O (sqrt(N)) per query, with a simple implementation.**

While that sentence above is fairly technical, let's jump into the details and make more sense of it!

-------------

Square root decomposition is used to solve range queries efficiently. It enables you to answer questions like "What is the smallest element between indexes L to R in an array?" or "What is the GCD of all elements between index L to R in an array?"

The manner in which this is done, is by some preprocessing.

What we do, is break up the input array into certain buckets.

Each bucket stores the "answer" of certain ranges in the array. 

These ranges in the array consist of sqrt(N) elements.

Thus, there exist ceil(N/sqrt(N)) buckets.


Let's take an example. 

> Let's consider an array A that consists of 16 elements.
> 
> Let's also attempt to find the sum of elements from index L to R.

We will first construct 4 buckets. Each bucket stores the answer for **4** (since sqrt(16) = 4) elements.

The "answer" here is the sum of elements in the ranges concerned.

The first bucket stores the sum of the first 4 elements. (Index 0 to 3)

The second bucket stores the sum of the next 4 elements. (Index 4 to 7)

The third bucket stores the sum of the next 4 elements. (Index 8 to 11)

The final bucket stores the sum of the final 4 elements. (Index 12 to 15)

![](../../img/square_root_decompo.JPG)

---------------

Now how would we answer a query?

In general, a query can be classified into at most three parts.

First, some suffix of elements in the array, corresponding to a bucket X.

Second, some subarray in my bucket array. (Some continuous buckets from (X+1) to some Y)

Third, some prefix of elements in bucket (Y+1).

------------

The first part has at most sqrt(N) - 1 elements.

The second part has at most sqrt(N) - 2 buckets.

The third part has at most sqrt(N) - 1 elements.

What this means, is that traversing these three parts in total can be done in O(sqrt(N)) and that's where the beauty of this algorithm lies.

The worst case query as mentioned above is described in the picture below.

![](../../img/square_root_decompo_worst.JPG)

The red line denotes the range of the query.

The blue lines denote which data structures to view for retrieving data.

--------------------

But how do we handle range updates?

Simple. Let's say we need to add X to all elements from index L to R.

All we need to do, is decompose the query range into the 3 sections as mentioned above.

The updates made on the suffix of the first bucket can be performed directly on the input array.

The updates made on the continuous buckets are stored in the bucket array.

The updates made on the prefix of the final bucket are performed directly on the input array.

The pseudocode for that would look as follows -

```python
left_bucket_idx = L/sqrt(N)

# update the first part
for j in range(L, sqrt(N)*left_bucket_idx):
	A[j] += X

right_bucket_idx = R/sqrt(N)

# update the second part
for j in range(left_bucket_idx, right_bucket_idx+1):
	bucket[j] += X

# update the third part
for j in range(right_bucket_idx*sqrt(N), R):
	A[j] += X

```

To retrieve the final array, we push all pending bucket updates onto the array.

That looks as follows -

```python
for i in range(N):
	bucket_idx = i/sqrt(N)
	A[i] += bucket[bucket_idx]

```

And that's it! Range updates and range queries in O(sqrt(N)) per query!

Obviously, point updates are also supported and they can be performed in the same way as a range update -- the key difference in the fact that only a single bucket is affected by a point update as opposed to a range update.

-------------------------------------------------------

For more of my work, follow me on:

LinkedIn: <span style="color:blue">[https://www.linkedin.com/in/adityaramesh1998/](https://www.linkedin.com/in/adityaramesh1998/)</span>

GitHub: <span style="color:blue">[https://github.com/RameshAditya/](https://github.com/RameshAditya/)</span>

Twitter: <span style="color:blue">[https://twitter.com/adityaramesh98](https://twitter.com/adityaramesh98)</span>

-----------------------------

### Practice!

The question that showed me the use of this was from a recent codeforces contest - <span style="color:blue;"><a href="https://codeforces.com/contest/1199/problem/D">https://codeforces.com/contest/1199/problem/D</a></span>

While most contestants either used a clever ad-hoc approach, or an overkill segtree+lazy propagation approach, this was my submission that fetched me an "Accepted" :) -

```c++
#include<bits/stdc++.h>
using namespace std;

#define ll long long

int main(){
	std::ios_base::sync_with_stdio(0);
	
	ll n;
	cin >> n;
	vector<ll> A(n);
	
	for(ll i = 0; i < n; i++)cin>>A[i];
	
	ll bucketsize = sqrt(n);
	vector<ll> bucket(bucketsize + 5);
	
	for(ll i = 0; i < bucket.size(); i++) bucket[i] = -1;		
	
	ll q; cin >> q;
	while(q--){
		ll type; cin >> type;
		if(type == 1){
			// push bucket into array and reset bucket
			ll p, x; 
			cin >> p >> x;
			ll bucketnum = (p-1)/bucketsize;
			for(ll j = bucketsize*bucketnum; j < min(n, bucketnum*bucketsize + bucketsize); j++) 
				A[j] = max(A[j], bucket[bucketnum]);
			bucket[bucketnum] = -1;
			A[p-1] = x;
		}
		else if(type == 2){
			// update all buckets
			ll val;
			cin >> val;
			for(ll i = 0; i < bucket.size(); i++) 
				bucket[i] = max(bucket[i], val);
		}
		
	}
	for(ll i = 0; i < n; i++){
		ll bucketnum = i/bucketsize;
		if(bucket[bucketnum] != -1) 
			A[i] = max(A[i], bucket[bucketnum]);
	}
	for(auto i: A) cout<<i<<" ";
}

```