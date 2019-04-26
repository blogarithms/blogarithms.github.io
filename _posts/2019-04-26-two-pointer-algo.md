---
layout: post
title: "The Two Pointer Algorithm!"
excerpt: "A really cool algorithm to solve some interesting problems."
categories: [algorithm]
image:
    feature: 
comments: true
---

I first ran into the two pointer algorithm a few months into beginning competing in online programming contests.

I'll jump right into what the algorithm is, and then we'll take a look at examples of where one can apply it.


This algorithm is often used in problems involving arrays.

In my experience of programming contests, I've found it to work as a great replacement to binary search, at times as well.

Let me show you an example, and that should make things clear as to what the actual algorithm is.

Consider the following problem -

> <b>1. Given a sorted array A of N elements, for each index i of the array, find how many elements in the array lie between A[i] and 2 * A[i] .</b>

Now, the simplest solution here is an O(N^2) solution where for each element, we iterate over every other element and see if A[i] <= A[j] and A[j] <= 2 * A[i], in which case we increment ans[i].

But we can do better.

A smarter algorithm would use binary search. For each A[i], we can find the largest index j such that A[j] <= 2 * A[i]. Then ans[i] = (j-i+1).

This algorithm has a time complexity of O(N * logN) since we're performing a binary search at every single element, so O(logN) * O(N) = O(NlogN) time, and O(1) extra space.

Normally, for a coding contest this should be enough - but let's say you were in an interview and you're asked to do better.

**This is where the two pointer algorithm can set you apart.**

Consider the following observation -
- Let i and j be two indexes of my array, where i < j.
- This obviously means A[i] <= A[j] (since the input array is sorted increasingly)
- Let's keep incrementing j until we reach some index where A[j+1] > 2 * A[i]
- Thus (j-i+1) is what we need to assign to ans[i]
- **Now here comes the cool part -**
- Clearly for our current i, we do not need to increment j forward, since we know we won't find any more valid elements, as all elements at further indexes will be greater than 2 * A[i] (sorted array, remember?)
- Also, we know that since the array is sorted, A[i+1] >= A[i] implying 2 * A[i+1] >= 2 * A[i].
- Thus, when we increment i to our next element, we also know that our current j, will be such that A[j] <= 2 * A[incremented i] (since A[j] <= 2 * A[old i])
- **This above line is crucial, make sure you understand WHY it works, by also reading the line above it.**
- Now that you know its correctness, you'll realize that we do not need to reinitialize j, and we can continue incrementing it while A[j] <= 2 * A[incremented i] (which is basically step 3 of this algorithm)
- Stop once i has reached the end.

Now if you're wondering how to analyze the time complexity of such an algorithm, let me put it to you this way -- is i ever being reinitialized?

No. It's only moving forward from 0 to N-1.

And is j ever being reinitialized? 

No. It's only moving forward from 0 to N-1.

We know the loop must end when i = N-1, and j = N-1.

Thus, both variables are only moving forward, implying an overall complexity of **O(N)**

Yeah, we just solved this in O(N) time and O(1) extra space! :)

A few takeaways here, is that the two pointer algorithm works only if -
- For a given VALID pair of indices (i, j), if (i+1, j) will also be valid, while (i, j+1) may be valid.

The validity of a pair is determined by the problem you're solving. In this case, the validity of pair (i, j) was defined by A[i] <= A[j] <= 2 * A[i]

---------------------------------------------------------

Let's look at another problem statement -

> <b>2. Given a string S of size N, find the number of substrings that have atmost K unique alphabets. </b>

Let's start simple here.

If we iterate over each substring, and count how many alphabets are in each substring, our answer will be equal to the number of substrings which have less than K unique alphabets.

The naive solution is O(N^3), since we must fix a starting point for our substring, an ending point for our substring, and iterate over the chosen substring.

This can be optimized to O(N^2) by fixing an i and then taking all prefixes of S[i:] and constantly checking whether the current prefix of this sliced string has K distinct characters or not -- which can be done by hashing the current characters seen.

Can we do better?

You know we can -- using the two pointer algorithm.

But this time I'll make things more formal --

The validity of a pair (i, j) here is defined as if len(set(S[i:j])) <= K.

That is, if the number of unique alphabets are <= K for the substring whose start and ends are (i, j) respectively.

Obviously, if (i, j) is a valid pair, then (i+1, j) is also a valid pair since no new alphabet must have been added in this range.

(i, j+1) may or may not be a valid pair however. 

Thus for a given i, we know there will exist some j such that (i, j) is valid and (i, j+1) is not valid.

From our observation, we know that (i+1, j) is valid, and (i+1, j-1) is also valid. Thus we only need to move j forward.

And we anyway move i only forward.

Every time we need to move i forward, this implies that the current (i, j) pair is the longest substring starting at i which contains at most K unique alphabets, which means that there are (j-i+1) substrings containing S[i], thus we increment our final answer by (j-i+1), and then increment i and repeat our algorithm.

And as far as checking whether a certain substring has at most K unique alphabets, we can preprocess the prefix sums of the 26 alphabets over the input string, after which checking the number of unique alphabets in a range can be done in O(1) time.

----------------------------------------------------------------

Try solving these next ones on your own, and you can also find my solutions on my GitHub, I'll link to that below.

> <b> 3. Find the number of pairs A[i] and B[j] from two arrays A and B, whose product is less than K. </b>

> <b> 4. Given a string S, for each index find the number of substrings with less than K <b>unique</b> characters that it is a part of. Here's what I mean - </b>
> 
> <b>For example, consider S = "abcdef" and K=2</b>
> 
> <b>Here, 'a' is a part of the valid substrings ("a", "ab") thus the answer for its index is 2.</b>
> 
> <b>Similarly, 'b' is a part of the valid substrings ("ab", "b", "bc") hence its answer is 3.
>
> <b>'c' too has an answer of 3 as it is part of these valid substrings - ("bc, c, cd") </b>





Find the code for all four of these challenges over on my GitHub here - <span style="color:blue;"><a href="https://github.com/RameshAditya/two-pointer-algorithm"> https://github.com/RameshAditya/two-pointer-algorithm </a></span>

There are other uses of the two pointer algorithm as well, such as determining cycles in linked lists, finding a pair of elements in an array that add up to X, or merging two sorted arrays into one sorted array.

All of them are worth thinking over.

Cheers!

----------------------------------------------------------------------------------------

For more of my work, follow me on:

LinkedIn: <span style="color:blue">[https://www.linkedin.com/in/adityaramesh1998/](https://www.linkedin.com/in/adityaramesh1998/)</span>

GitHub: <span style="color:blue">[https://github.com/RameshAditya/](https://github.com/RameshAditya/)</span>

Twitter: <span style="color:blue">[https://twitter.com/adityaramesh98](https://twitter.com/adityaramesh98)</span>