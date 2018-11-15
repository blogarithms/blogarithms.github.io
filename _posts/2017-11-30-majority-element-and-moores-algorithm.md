---
layout: post
title: " Majority Element and Moore's Algorithm!"
excerpt: "Find an array element occurring more than half the time in O(n)!"
categories: [algorithm]
comments: true
---
<i>This is a short, bite-sized algorithm since I'm writing this on my phone in a free class. Soo. Let's get on with it! </i>

<hr>
Consider the question, "Given an array of <i>n </i>elements, find out which number occurs more than <i>n/2 </i>number of times, if any. If none, print -1."

Catch is, <b>do it in O(n) time and O(1) additional space. </b>

First, let's discuss other potential (but worse) solutions. Feel free to skip ahead to the section titled "Moore's voting algorithm" if you want to skip to the action.

<hr>

<h2>O(n^2) time and O(1) space approach</h2>
Now, let's go through the naive solution -

The algorithm here is straightforward -
<ul>
<li>Declare two integers - <i>answer, answer_frequency </i></li>
<li>Run a nested loop iterating over all elements, for every element.</li>
<li>Find if any element occurs more than <i>n/2</i> number of times.</li>
<li>If yes, break out and print accordingly.</li>
<li>Else, print -1.</li>
</ul>
Simple and clear. But we want better.



<h2>O(nlogn) time and O(1) space approach</h2>
This approach is simple too.

It goes as follows -
<ul>
<li>Sort the array (either ascending or descending.)</li>
<li>Find the element that repeats continuously more than <i>n/2</i> number of times.</li>
<li>Print element, else print -1.</li>
</ul>
Sorting the array in-place uses O(1) additional space and using a good sorting algorithm (merge sort or introsort) should get you an O(nlogn) time complexity.

But let's do even better.



<h2>O(n) time and O( max(A) ) space approach</h2>
Now, lets say that we knew the values of the elements in the array are less than 100,000.

Yay! This means we can hash them in a direct address table!
<ul>
<li>Create a hash table <i>freq[] </i>where <i>freq [x] </i>stores the frequency of the number <i>x.</i></li>
<li>Iterate over the array <i>freq</i> and check if any <i>freq[ i ] </i>exceeds <i>n/2</i>.</li>
<li>Print the element that does so, else print -1.</li>
</ul>
Of course, this depends on the values in the array so it's fairly risky to apply it when you don't know the upper bound on the elements.

Let's do even better.

<hr />

<h2>Moore's Voting Algorithm - O(n) time and O(1) space approach</h2>
Alright, let's top it all. Mathematically it's easy to see that to even solve a problem like this, you're required to at least see all elements in the array at least once. 
<br><br>
<b>Therefore you cannot have a better time complexity than a linear complexity here</b>.
<br><br>
This is the best solution.

Let's dive in.

The algorithm is as follows -
<ul>
<li>Declare two integers - <i>majority_index </i>and <i>count</i></li>
<li>Assign <i>majority_index </i>to <i>0 </i>and <i>count</i> to 1. (What's happening here is that <i>majority_index </i>always stores the index of the majority element in the array, and the <i>count</i> variable keeps track of this number's frequency.
Now we run a loop, from index <i>1</i> to <i>n-1 </i>(0-based indexing)</li>
<li>Now there are two cases -</li>
<ul>
	<li>Case 1: <b><i>A[ current_index ] = A[ majority_index ]</i></b><i> </i>in which case we simply increment the <i>count</i> variable as our expected majority number has occurred once more.</li>
	<li>Case 2: <b><i>A[ current_index ] != A[ majority_index ] </i></b>where we decrement the <i>count</i> variable by one as another variable seems to have occurred while we expected our majority element to have occurred.</li>
</ul>
<li>Now at every iteration of our loop, after these 2 cases we check if <i>count</i> has equaled zero.</li>
<li>If it has equaled zero, we update <i>majority_index</i> to the <i>current_index </i>and update <i>count </i>as 1. As if we're assuming this new current number could be the majority element.</li>
<li>Now that we're done by our <em>probable</em> element, we re-iterate over the array to confirm its count exceeding <em>n/2</em></li>
</ul>
And that's it! At the very end of the loop, if <i>counter </i>exceeds <em>n/2</em>, print <i>A[majority_index] </i>else print -1.

The implementation of this algorithm (in Python) is as follows -
<hr>
<pre><span style="color: #0000ff;">#Phase 1: finding a probabilistic majority element</span>

<span style="color: #333333;">majority_index=0</span>
<span style="color: #333333;">count=1</span>
<span style="color: #333333;">for i in range(1,n):</span>
<span style="color: #333333;">     if A[i]==A[majority_index]:</span>
<span style="color: #333333;">          count+=1</span>
<span style="color: #333333;">     else:</span>
<span style="color: #333333;">          count-=1</span>
<span style="color: #333333;">     if count==0:</span>
<span style="color: #333333;">          majority_index=i</span>
<span style="color: #333333;">          count=1</span>

<span style="color: #0000ff;">#Phase 2: verifying the majority element
<span style="color: #333333;">count=0
</span><span style="color: #333333;">for i in range(n):
</span>     <span style="color: #333333;">if A[i]==A[majority_index]:</span>
<span style="color: #333333;">          count+=1</span>

<span style="color: #333333;">print(A[majority_index]) if count>=(n>>1) else print(-1)</span></span></pre><hr>
That's how you find an element that occurs more than <i>n/2 </i>times in an array of length <i>n </i>in O(n) time and O(1) space!

<hr />

<i>Oh look at that, I managed to type this all out and the teacher's still not started teaching. Yay!</i>

<i>Anywho, that's Moore's voting algorithm for you. </i>

<i>Hope it helped. </i>

<i>Later! </i>

<i>- Aditya</i>