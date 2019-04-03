---
layout: post
title: "Interviewing students on Algorithms for my college's ACM chapter."
excerpt: "As a CS pre-final year undergrad, I get to be one of the 'gatekeepers' to my college's ACM chapter."
categories: [algorithm, personal]
image:
    feature:
comments: true
---

So I'm part of my college's ACM Chapter, and last week I got to interview freshmen and sophomores who were interested in joining the college's ACM chapter.

Personally, I don't think I got enough time to judge each candidate due to the overwhelming participation, and so I decided to write this blog post to share with readers the fancy algorithmic challenges I prepared -- which I sadly never got the time to ask candidates.

So, here goes! I've split up the article into 3 sections, **Easy**, **Medium** and **Hard**.

Aside from my question, I'll also leave a one or two-line solution as a comment that gives the direction or approach required to solve the problem.

# Difficulty Levels
- [Easy](#easy) 
- [Medium](#medium)
- [Hard](#hard)

---

## Easy
#### Question 1

Given an array whose elements are initially strictly increasing, and then strictly decreasing, find the maximum element in ```O(logN)``` or better.


> I'd solve this using binary search. The idea is, for a given range, to check the middle element and the next element (if one exists).
> 
> In general, if the next element is greater than the current element, then one knows that the maximum element is yet to be encountered -- else it has already been encountered and lies to the left of the current 'mid'.
> 
> Hence, a simple modified binary search was expected.

#### Question 2 

You're the driver of a snowplower in a city whose roads are filled with snow. Houses are connected by roads and there always exists some route between any two houses in the city.

Any time you pass through a road, you clear out all the snow in it and clean up the road. What is the minimum amount of snow you need to clear, to visit all houses?

> The idea is simple here. First, it's obvious that this is a graph theory problem. Roads are treated as edges, and the edge weights would be the amount of snow on each road. 
> 
> Since the snow is cleared up after the first pass through the road, you could select an edge, and then update its edge weight to zero.
> 
> The problem then simply resolves down to finding the minimum spanning tree of a graph - accomplished by either Kruskal's algorithm or Prim's algorithm.

#### Question 3 

Calculate X^Y (exponentiation) in better than ```O(logY)```.

> The test here is simply to see if the candidate could realize how repeated squaring can save operations.
> 
> For example, x^8 = (((x^2)^2)^2)
> 
> The complete algorithm (binary exponentiation) is also available as a blog post here on my blog. :)

#### Question 4 

Given Q numbers, for each number determine the number of prime factors in ```O(Q + NlogNloglogN)``` or better.

> I expected the candidate to approach the problem with a sieving strategy, or any other such optimal preprocessing approach.

#### Question 5 

Given a string of size N and another integer K, determine the largest substring with less than or equal to K unique characters in ```O(NlogN)``` or ```O(N)```.

> I've solved similar questions using a hashmap and a variable together, to keep track of the current number of distinct characters in a certain 'window', whose size would be determined by a two-pointer technique.
> 
> The algorithm would consist of two pointers, 'left' and 'right', and in general, keep incrementing 'right' while the number of distinct characters are <= K.
> 
> If the number of distinct characters > K, then keep incrementing 'left' while this condition is true.
> 
> Repeat the above two steps until 'right' reaches the end of the string, and also keep track of the maximum value of (right-left+1) and the corresponding left and right values, which will be the final answer.

---

## Medium 

#### Question 1

A simple problem on difference arrays. Find the blog post on my blog here. :)

#### Question 2 

Given an array A of N numbers, and another array B of M numbers, return an array C of M numbers where C[i] = maximum value of (B[i] XOR A[j]) (where j can vary 1 to N)

Do this in ```O(M * log(max(A)))``` or better.

> A pretty common use of tries, by inserting the binary representation of numbers into the trie and following a greedy approach.

#### Question 3

Given an array A of N numbers, and Q queries each of the form "L R" where L and R are integers, find the number of even numbers between indexes L and R for each query in ```O(QlogN)``` or better.

> Simple segment tree / fenwick tree problem. Each node in the tree stores number of even elements corresponding to the relevant range in the input array.
>
> Query over required ranges and add up the contribution of each required node.

#### Question 4

Given an array A of N numbers, find the number of triplets such that A[i]<A[j]<A[k] and i<j<k in ```O(N*logN)``` or better.

> The idea is clever here. Use a binary indexed tree as a frequency hashmap for each element, and for every element in the array, do: final answer += (number_of_elements_less_than_Ai) * (number_of_elements_more_than_Ai) which takes O(logN) to calculate using the fenwick/binary-indexed tree.

#### Question 5 

Given a map of multiple treasures and the roads connecting these treasures, you want to visit each treasure and collect all the gold they contain.

But each road has some bandits that steal your money or put you in debt if you don't have enough money to pay them yet.

What is the most money you can make after visiting all treasures?

> Treat this as a graph theory problem -- each node value is the treasure's value, and each edge has a negative value equal to the amount of money that the bandits would steal from you.
>
> Find the maximum spanning tree, and then add the value of those edges to the value of all nodes and print the sum.

#### Question 6

Given a string of size N and another integer K, determine the **number of substrings** with less than or equal to K unique characters in ```O(NlogN)``` or ```O(N)```.

> Read Question 5 of the 'Easy' section. The twist is to just update the answer by the value of (Right-Left+1) for every time you increment Right, as every time you increment Right, you add (Right-Left+1) new suffixes to the current substring in the window.

---

## Hard 

#### Question 1 

Given a tree where each node is assigned a value (positive or negative), find the largest sum that can be obtained by adding up the values of a subset of nodes where each node can reach another in the subset (i.e. a connected subset of nodes)

> Dynamic Programming. For every node, calculate the maximum value of its children including the current node. 
>
> Then assign dp[node] = max(dp[node], dp[node] + dp[subset_of_children])

#### Question 2 

Given a list of words that are supposedly arranged in dictionary order, determine if there exists a valid permutation of the english alphabet in which the given list of words are indeed arranged alphabetically.

Example, [bac, dac, aac, afc]

Accepted answer: Any permutation of the english alphabet where 'b' appears before 'd', and 'd' appears before 'a', and 'a' appears before 'f'.

> Topological sort. Each letter is treated as a node and there exists a directed edge between every adjacent letter. There also exists a directed edge between the distinct letters right after the longest common prefix between every adjacent word.
>
> Meaning, for the above example, the edges are b->d, d->a, a->f 
>
> All other nodes are just single nodes with no edges. Thus any valid topological sorting of the graph is an answer.

#### Question 3

Given a string, find the largest 'factor' of the string. A 'factor' of a string is defined as a substring which can generate the original string by repeatedly concatenating to itself in ```O(N)``` or better.

Example, 'abababab' has the largest factor 'abab' as when written twice, it can generate the original string.

> Z-algorithm. Find the Z-vector (largest common prefix of current suffix with original string).
>
> If z[i]+i == N, then i is a valid answer.