---
layout: post
title: "Dynamic Programming: Sum Over Subset"
excerpt: "A small yet cool algorithm, that helps iterate over numbers whose bits are a subset of its parents. "
categories: [algorithm]
comments: true
---
Back! Finally! Things got real, real busy. Ah the hectic life I tell ya.</em>

<em>Anywho, 'nuff stalling. </em>

<em>So I discovered this neat algorithm a couple of weeks ago while reading the editorial to the problem <a href="https://www.codechef.com/problems/MAXOR">CODECHEF - MAXOR</a>, and during the contest, I just implemented the brute-force for a quick 20 pts, but couldn't figure out the 100pt algorithm in time and that's the inspiration for this article here, and so is <a href="http://codeforces.com/blog/entry/45223">this codeforces blog article</a>. <strong>Dynamic Programming</strong>, mixed in<strong> with bit masking</strong>, what could go wrong? :)</em>

<em>Let's dive in!</em>

<hr>

<h2>Basics of bits</h2>
<em>x << y </em> : Shifts x by y bits to the left.

x & (1 << y) : Checks if the y<sup><em>th  </em></sup>left bit is set in x or not.

x l (1 < < y) : Sets the y<sup><em>th  </em></sup>left bit in x.

x ^ (1 << y) : Flips the y<sup><em>th </em></sup>left bit in x.

<hr>

<h2>Incorporating DP</h2>
Okay, so consider the  following question -

<em><span style="color:#ff0000;"> Given a fixed array <strong>A</strong> of n integers, we need to calculate ∀ x function <strong>F(x)</strong> = Sum of all <strong>A[i] </strong>such that <strong>x&i = i</strong>, i.e., <strong>i</strong> is a subset of <strong>x</strong>.</span></em>

Now, effectively what the question asks of you is to find the sum of all values consisting of pairs where one of the numbers is an entire subset of the other number, in terms of set bits.

To use this method, initially add zeroes to the given array of n integers to get the size to a power of 2. Call this 2^N, with N being the power. The following applies after.

In order to do this, we use a <em>dp</em> array where <em>dp[i][j] </em>stores the answer across all values in <em>A</em> until the <em>(j+1)th </em>bit from the right in all numbers.

The flowchart below, each non-leaf node is of the form (binary, bit index from right considered) that makes the visualization of the data easier.

<img class=" aligncenter" src="http://codeforces.com/predownloaded/b2/de/b2deb315ff5f2d3ecc27eea26f2ae2e5d10d47c3.png" alt=" " />

That is, <em>dp[mask][i] </em>would store the result for the sum of all numbers up till bit number <em>i</em> from the right as obtained using <em>mask</em> as a bitmask.

Thus, corresponding dp indices will only be matched when the <em>i</em>th bit is set in mask, where mask iterates over all possible numbers that can be formed (0 to 1<<20) in the case of standard problems.

Thus the implementation is as follows -

```c++
for(int mask = 0; mask < (1 << N); ++mask){
   dp[mask][-1] = A[mask]; //handle base case separately (leaf states)</span> 
        for(int i = 0;i < N; ++i){ 
             if(mask & (1<<i)) 
                  dp[mask][i] = dp[mask][i-1] + dp[mask^(1<<i)][i-1]; 
             else 
                  dp[mask][i] = dp[mask][i-1]; 
        } 
   F[mask] = dp[mask][N-1];
}
```


Now let's explain the code -

Firstly, we handle the leaf states where <em>dp[x][-1]=A[x], </em>as there is no change between <em>dp </em> and <em>A.</em> Having done this, we go to the next step. Iterating over the other bits of the given array of numbers.

The idea of the <em>if</em> case here, is that <strong>if mask & (1<<<em>i</em>) == 1, </strong>then that means the <em>i</em>th bit is set in <em>mask</em>, meaning this value could contribute to the current mask as well, and thus the sum increases and <em>dp[mask][i]</em> is incremented by the value in the mask with the ith bit turned off.

Thus --

If the <em>i</em>th bit is set, the value of dp[mask][i] must consider both, dp[mask][i] AND dp[mask ^ (1<<i)][i], the state wherein the <em>i</em>th bit is NOT set.

Else, if the <em>i</em>th bit is off here, one can NOT add the value where the <em>i</em>th bit is on, as then it would no longer store a subset.

To summarize:

<em><img class="tex-formula" src="http://codeforces.com/predownloaded/cd/89/cd89346ca587936aa5d3659676a3146ab8c33658.png" align="middle" /></em>

The above recurrence, upon further analysis can be space-wise optimized into the following snippet:

```c++
for(int i=0;i<(1<<N);i++)new[i]=A[i];

for(int i=0;i<N;i++)
  	for(int mask=0;mask<(1<<N);mask++)
       		if(mask&(1<<i))
			new[mask]+=new[mask^(1<<i)];
```

Personally, the hardest part of understanding this is understanding the flow of the code, that is, how the flowchart is filled and the intuitive proof that the subcases have already been solved for through the iterative procedure. Anyway, grasping this was satisfying.
<br><br>
And that's that!
<br><br>
I may add in an explanation to <a href="https://www.codechef.com/problems/MAXOR">MAXOR</a> later on, too, but that's if I find the time to do so, 'cause I'm swamped with assignments, projects and college work.
<br><br>
Oh well, until next time!
<br><br>
<em>Aditya</em>