---
layout: post
title: "Dynamic Programming on Trees - In Out DP!"
excerpt: "A nice concept that allows you to solve cool problems involving DP on trees!"
categories: [algorithm]
image:
    feature: 
comments: true
---

Consider the following problem -

Given a tree, for each node, output the distance to the node farthest from it.

Example:

1->2

1->3

Output: 
1 2 2

Explanation:
The first element of the output array is 1 because node 2 or node 3 is one edge away from node 1.

The second element of the output array is 2 as node 3 is two edge lengths away from node 2.

The third element of the output array is 2 as node 2 is two edge lengths away from node 3.

Simple solution:

Let's rephrase the problem to the following -

Given a tree rooted at a certain node, find the distance to the leaf node farthest from it.

Now, if were to root the tree at each possible node, and solve the above, we can generate our output array.

But this requires a DFS from each node, to generate the entire output array. Thus it's an O(N^2) solution.

But maybe its possible to do better.

If we know the answer of a certain node, we can be sure that for the child of this node, the leaf node farthest from the child will either be in the subtree of the child, or in the subtree of the siblings of this child, or outside the subtree of the current node. (Obviously, as these 3 possibilities cover the full tree)

This is of key importance as this allows us to speed up our solution to O(N).

Note the following -

```python
ans[child] = max(farthest_dist[leaf_in_child_subtree], farthest_dist[leaf_in_sibling_subtree], farthest_dist[leaf_outside_node_subtree])
```

The issue is now to compute this "farthest_dist" array inside subtrees and outside subtrees.

This is where in-out DP comes in.

------------------------------------

The idea behind in-out DP is to generate two arrays in a preprocessing step - `in` and `out`

`in` is an array that stores valuable information of the subtree of a node.

`out` is an array that stores valuable information of the portion of the tree outside the subtree of a node.

In this case, `in` and `out` store the farthest distance to a leaf node for a given current node.

Let's look at how we compute these arrays now -

To compute `in[node]`, we need to find the distance to the farthest leaf node inside the subtree of `node`.

This is easily done by a DFS, and the DP recurrence is -

```python
in[node] = max(in[node], 1+in[child])
```

After computing `in`, we now need to compute `out`.

This is slightly trickier, as we're considering all the nodes that are OUTSIDE the subtree of the current node.

But we can decompose this section of nodes as - (out[parent] + in[siblings])

![DPIO](https://i.imgur.com/0FYyar8.jpg)

The diagram illustrates this.

Consider the case when you're computing `out[green_node]`

The solution space for `out[green_node]` is the union of the spaces `in[blue_nodes]` and `out[parent]`

The blue nodes are siblings of the green node, and all nodes to the right of the purple border are considered in `out[parent]`.

Hence we can verify the correctness of our approach.

Thus, the recurrence relation is -

```python
out[node] = max(2+in[siblings], 1+out[parent])

# Look at the diagram if you cannot identify why there exists a "2+" and "1+" in the above equation
``` 

And there we have it!

But we still need to perform another optimization.

When computing `in[siblings]`, its optimal to preprocess them for each node and only maintain the top 2 values, so that for each node at the same level, we don't re-process nodes.

The reason we need to select TWO values and not just the best, is as for some particular child, the optimal answer would've been present inside its own subtree - therefore we need to consider the second-best value as the optimal `in[sibling]` value for this specific child.

The below code should solve the question at the beginning of the article -

```c++
#include<bits/stdc++.h>
using namespace std;

#define ll long long

const int N = 3e5;

vector<ll> g[N];

int in[N], out[N];

// compute in[] array
ll dfs1(ll node, ll par){
	in[node] = 0LL;
	for(auto child: g[node]){
		if(child == par)continue;
		dfs1(child, node);
		in[node] = max(in[node], 1+in[child]);
	}
}

// compute out[] array
ll dfs2(ll node, ll par){
	ll mx1 = -1;
	ll mx2 = -1;
	
	for(auto child: g[node]){
		if(child == par) continue;
		if(in[child] >= mx1) mx2 = mx1, mx1 = in[child];
		else if(in[child] > mx2) mx2 = in[child];
	}
	
	for(auto child: g[node]){
		if(child == par) continue;
		ll use = mx1;
		if(mx1 == in[child]) use = mx2;		
		out[child] = max(1LL+out[node], 2LL+use);
		dfs2(child, node);
	}
}

int main(){
	std::ios_base::sync_with_stdio(0);
	memset(in, 0, sizeof(in));
	memset(out, 0, sizeof(out));
	ll n;
	cin >> n;
	for(ll i=0;i<n-1;i++){
		ll u, v;
		cin >> u >> v;
		g[u].push_back(v);
		g[v].push_back(u);	
	}
	dfs1(1, 0);
	dfs2(1, 0);
	
	for(ll i=1;i<=n;i++) cout << max(in[i], out[i]) <<" ";	
}

```

Now try out another problem on your own (whose solution I've enclosed below anyway) - 

Given a tree, for each node, find the sum of distances to every other node, in linear time complexity.

Example - 
1->2

1->3

Output: 
2 3 3

Explanation:
For node 1, 1->2 + 1->3 = 1+1 = 2

For node 2, 2->1 + 2->3 = 1+2 = 3

For node 3, 3->1 + 3->2 = 2+1 = 3

Hint: Let `in` store the sum of distances to each node in the subtree of the current node, and `out` store the sum of distances to all nodes outside the current node's subtree. We may also need another array that tells us the number of nodes in a certain subtree.

Solution:

```c++
#include<bits/stdc++.h>
using namespace std;

#define ll long long

const int N = 3e5;

ll n;
ll dp[N], in[N], out[N];
vector<ll> g[N];

ll predfs(ll node, ll par){
	dp[node] = 1;
	for(auto child: g[node]){
		if(child == par) continue;
		predfs(child, node);
		dp[node] += dp[child];	
	}
}

ll dfs1(ll node, ll par){
	in[node] = 0;
	for(auto child: g[node]){
		if(child == par) continue;
		dfs1(child, node);
		in[node] += dp[child]+in[child];
	}
}

ll dfs2(ll node, ll par){
	for(auto child: g[node]){
		if(child == par) continue;
		out[child] = (in[node] - in[child] - dp[child]) + out[node] + (n-dp[child]);
		dfs2(child, node);
	}
}

int main(){
	std::ios_base::sync_with_stdio(0);
	cin >> n;
	for(ll i=0;i<n-1;i++){
		ll u, v;
		cin >> u >> v;
		u--, v--;
		g[u].push_back(v);
		g[v].push_back(u);
	}
	
	predfs(0, -1);
	dfs1(0, -1);
	dfs2(0, -1);
	for(ll i=0;i<n;i++) cout << in[i] + out[i] << " ";
}

```

----------------------------------------------------------------------------------------

For more of my work, follow me on:

LinkedIn: <span style="color:blue">[https://www.linkedin.com/in/adityaramesh1998/](https://www.linkedin.com/in/adityaramesh1998/)</span>

GitHub: <span style="color:blue">[https://github.com/RameshAditya/](https://github.com/RameshAditya/)</span>

Twitter: <span style="color:blue">[https://twitter.com/adityaramesh98](https://twitter.com/adityaramesh98)</span>
