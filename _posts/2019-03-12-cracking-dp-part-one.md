---
layout: post
title: "The One On Dynamic Programming!"
excerpt: "Probably the first in a multi-part series on DP."
categories: [algorithm]
image:
    feature: 
comments: true
---

Dynamic programming (DP, as I'll refer to it here on) is a toughie.

I've heard a lot of friends and juniors complain about dynamic programming and about how non-intuitive it is.

Plus, problems on DP are pretty standard in most product-company-based hiring challenges, so it seems like a good topic to address on a blog based on algorithms.

Here's a heads-up to you though, since this article is the first in (maybe, depending on its reception), a multi-part series on DP, this article will focus on setting the groundwork of DP and I'll dedicate most of this article to explaining how to arrive at the solutions to classical DP problems, and the general pattern of thought that I use when it comes to DP.

This article, I'll be tackling the following -
- Coin Change Problem
- 0-1 Knapsack Problem
- DP on grids

And I'll be leaving some more classical variants of DP as exercises. 

If you're already fully confident of why/how the above DP algorithms work correctly, then reading this article may yield little value, and you may be better off waiting for the next article on DP. :)

Should I add future articles, I'll tackle some non-classical variants of DP, leveraging intuition and logic.

Onto the article!

### DP: An Intro

I've found myself solving the nicest of DP questions when I leverage recursion. 

This is something I've often noticed in general, that for me, recursion is very intuitive.

While DP at times does seem confusing, I've found from personal experience that thinking of solutions and problems recursively makes reaching a DP solution that much easier.

Let's take an example -

##### The Coin Change Problem

Let's say you have an infinite amount of `K` different denominations of coins.

You need to find the minimum number of coins to possess a value of N dollars.

For example, if `K = 2` and your denominations are `{1, 2}`, and you want to find the min. number of coins needed to generate `N=9` dollars.

Here, it's pretty obvious that'd be `5` coins (`4` two dollar coins + `1` one dollar coin).

> Note: A greedy algorithm of selecting the largest available denomination under `N` at every instant won't give optimal answers.
>
> Try out the test case: `K=3`, `denominations = {1, 3, 4}` and `N = 10`.
>
> The greedy answer yields 4 coins (`4` + `4` + `1` + `1`) while the optimal answer is 3 coins (`4` + `3` + `3`).

Thus, clearly we need to check all possible coin selections we can make.

Now let's model the problem recursively.

Let's define a mathematical function `f` where `f(N)` tells me the minimum number of coins I need to generate a value of `N`.

Let's say I have `K` denominations.

Now, if I were to take away a coin from my value of `N`, this coin could be of any denomination, correct?

Basically, `N` could be re-written as `((N-denom[i]) + denom[i])` for any valid `0<=i<length(denom)`.

I also know, that `denom[i]` can be represented as 1 coin (a coin whose value equals `denom[i]`).

Thus, `f(N) = f(N-denom[i]) + 1`

This is our general case.

What's our base case?

If `N=0`, we need 0 coins.

Similarly, if `N<0`, its an impossible case, which needs to be handled.

Compiling all this, we have:

```python

>>> def f(n, denom):
	ans = 10**10
	if n<=0:
		return 0
	for i in denom:
		if n-i>=0:
			ans = min(ans, 1+f(n-i, denom))
	return ans

>>> f(10, [1,3,4])
3

```

Yay! 

Now there are a couple of things we need to ensure we're handling, if we want to port this to DP.

- No global variables should be modified in the function
- Simple, memorizable states (state-space reduction)

And... I'd say that's it?

Now, we can make `denom` a global array as we're not changing it.

This makes our function just have one parameter. The current value, `N`.

Thus, we have an O(N*K) DP solution to this, as follows -

```python

>>> dp = [-1 for i in range(n)]
>>> def f(n):
	if dp[n]!=-1:
		return dp[n]
	ans = 10**10
	if n<=0:
		return 0
	for i in denom:
		if n-i>=0:
			ans = min(ans, 1+f(n-i))
	dp[n] = ans
	return ans

```

And that's it!

------------

##### The 0-1 Knapsack Problem

Okay, so let's say you're a burglar who has a bag (knapsack) that can carry a total weight of `W`.

There are `N` valuable objects in the house you broke into.

Each object `i`, has a weight of `wt[i]` and a value of `vals[i]`.

Your task as a burglar is to devise an efficient algorithm that can maximize the net value of objects you can steal from the house provided you place them all in your knapsack.

> I hope you don't have to apply this in seriousness. :P

So, let's think recursively here.

In general, whenever problems involve multiple solutions where there's an "option" to choose or ignore an element, I find it suitable to use the current index of the element as a parameter.

So this means that my `f()` method already has one parameter - `int idx`

Now let's think about what our other parameter could be by simulating our burglary.

At any point, when we're looking at the object at index `idx`, we have a choice - to take it, or to not take it.

The point of DP is to optimize our solution of "checking all possibilities", without repeating checking already checked possibilities, right?

So, we have a choice -- take the current object, or ignore it.

But let's say we do take it -- we still need to verify if our knapsack can hold it, right?

Lets look at this recursively.

To make sure our current possibility is valid, and to provide insight into our decision as to whether or not can we afford to take the next object, we NEED to know our current knapsack capacity that's left.

So, our states are (current_index, weight_left).

Let's see how we can model this as a recursive equation now -

```python

f(current_index, weight_left) = max( f(current_index + 1, weight_left), vals[current_index] + f(current_index+1, weight_left - wt[current_index]) )

```

Look closely to the right-hand-side of that equation.

We're stating:

f(idx, weight) = max( possibility where we don't take current element, possibility where we do take current element )

It's important to note that we've defined `f(idx, weight)` to be the maximum value we can obtain assuming we're at index `idx` with weight `weight` left over.

Also, pay attention to the states in the latter possibility, we even update the `weight_left` parameter to reflect the fact that we're picking the current object.

Here's how that looks: (I've accidentally switched the order of the states, but that doesn't matter)

```python

def brute(weightleft, idx):
    if idx >= n:
        return 0
    if dp[weightleft][idx] != -1:
        return dp[weightleft][idx]
    ans = 10**10
    ans = brute(weightleft, idx+1)
    if wts[idx] <= weightleft:
        ans = max(ans, vals[idx] + brute(weightleft - wts[idx], idx+1))
    dp[weightleft][idx] = ans
    return ans

# call the function as brute(W, 0)

```

-----------------

#### DP On Grids

This is a pretty common topic under DP.

Consider the following example -

Given an N x M grid, with each cell containing some gold in it, find the maximum amount of gold you can collect, given the following criteria -

- You start at the top-left corner
- You must end at the bottom-right corner
- You can only move to the cell at your right, or to the cell directly below you

Now, given these conditions and information, we need to come up with an O(N*M) solution to solve this problem.

Again, let's think recursively.

Below I've drawn a simple 3 x 3 grid, and labelled each node from 1 to 9.

1 2 3

4 5 6

7 8 9

*Note, the numbers above are LABELS and not values, they are only used to identify the cells I'm talking about*

Now, look at cell number 5. Since I can only move downward or rightward, the only way **to get to cell number 5** is either if I'm coming from cell number 2 or cell number 4.

Similarly, consider cell number 8. The only way to get to cell number 8 is if I'm coming from cell number 7 or cell number 5.

And in any case (excluding the 1st row or 1st column), I'm visiting a cell either from it's upper neighbor or its left neighbor.

This means, that for any node, the maximum value I can collect when I reach it is -

f(node) = val[node] + max(f(upper_neighbor), f(left_neighbor))

And that's it!

That's our recursive equation!

So let's formalize this in code, and this time, let's do this in a bottom-up tabulation manner.

```python

dp = [[0 for i in range(m)] for j in range(n)]

dp[0][0] = A[0][0] # the top left element has a max value of itself

for i in range(n):
	for j in range(m):

		# first row
		if i==0 and j!=0:
			dp[i][j] = dp[i][j-1] + A[i][j]
	
		# first column
		elif i!=0 and j==0:
			dp[i][j] = dp[i-1][j] + A[i][j]
	
		# general case
		else:
			dp[i][j] = A[i][j] + max(dp[i-1][j], dp[i][j-1])

print(dp[n-1][m-1])

```

Here's a few homework tasks for you to try out.

They're fairly simple, and I'll explain them a bit maybe in the next DP article anyway, so here they are -

Given an N x M grid, find the number of different paths one could take if they start at the top-left corner and end at the bottom-right corner and only move either down or right from each cell. 

- Do this in O(N*M)
- Bonus: Do this in O(N+M)

------

To summarize -

There are two aspects to DP -
- Identifying your states
- Identifying your recurrence relation with those states

Personally, I find it easier to start with determining the recurrence relation, and then trying to lower the states I need to fully track all information I have.

Hopefully this article helped make DP a bit more intuitive.

I plan on orienting the next article more toward the applications of dynamic programming on strings or on graph theory, depending on how well this article is recieved. :)

Cheers!

----------------

For more of my work, follow me on:

LinkedIn: [https://www.linkedin.com/in/adityaramesh1998/](https://www.linkedin.com/in/adityaramesh1998/)

GitHub: [https://github.com/RameshAditya/](https://github.com/RameshAditya/)

Twitter: [https://twitter.com/adityaramesh98](https://twitter.com/adityaramesh98)