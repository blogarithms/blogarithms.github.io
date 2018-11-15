---
layout: post
title: "Diving Into Disjoint Sets!"
excerpt: "Explore union-find algorithms with this data structure!"
categories: [algorithm]
comments: true
---
<i>Alice has two friends, Bob and Charlie. Now, the three of them decide to meet up at a coffee shop. </i>
<br><br>
<i>Bob, now runs into a friend of his, Dexter. </i>
<br><br>
<i>And Charlie runs into a classmate of his, Esther. </i>
<br><br>
<i>Bob and Charlie invite their friends over to the same table. The size of the crowd goes from 3 to 5 and Alice greets her extended friends Dexter and Esther. </i>
<br><br>
<i>As the day progresses, Dexter sees a couple of his colleagues at another table and switches tables to join their table. </i>
<br><br>
<i>Can you think of an algorithm to store this data optimally, one which can tell you if two given people share the same table, are friends, extended friends or strangers to one another? </i>
<br><br>
<i><b>Cue Disjoint Sets. </b></i>
<br><br>
In more formal terms, Disjoint sets (aka Union-Find) is a data structure that allows you to <i>connect </i>components and form larger components allowing you to efficiently handle queries asking "who is connected to who".
<br><br>
Even better, is that the most efficient form of their implementation which I use (which is what we'll be discussing here) is extremely small (~2 lines)
<br><br>
So! Having said that, let's dive in!
<br><br>
This data structure is implemented with an auxiliary array that keeps track of a group's leader. I'll explain what this means in the following paragraphs.
<br><br>
Now, before we go further, let's understand how we'd solve this without disjoint sets.
<br><br>
In the above example, the friends would represent the nodes of a graph and edges between nodes would indicate that those nodes (friends) were sharing a table. Queries could be handled by a simple breadth-first-search then.
<br><br>
But let's try a different approach. Consider this example, where we introduce a formal term.
<br><br>
<b>Let each table have a "leader", a person whom </b><b><u>only</u></b><b> everybody on that table "admires'. </b>
<br><br>
<b>Now if two people have the same leader, it would imply they share the same table. </b>
<br><br>
This is the concept of Disjoint Sets.
<br><br>
An array <i>parent</i> or <i>leader </i>defines the parent for every element, as is done in an hash map.
<br><br>
So a disjoint set is not a unique data structure but a way of thinking.
<br><br>
Its implementation involves only two methods -
<br><br>
<b>join( )</b>
<br><br>
<b>find( )</b>
<br><br>
These two suffice in performing standard operations on Disjoint sets.
<br><br>
<hr />
<br><br>
<h2>join( )</h2>
This function is used to combine two disjoint sets. i.e. to <b>unite</b> them. Now, to maintain consistency, a certain heuristic (I'll explain what this means in a moment) is followed to decide when joining two sets, which set is joined to which -- i.e. who stays the overall leader and which leader loses his leadership status.
<br><br>
This heuristic depends is circumstantial. Generally, in terms of optimization, it's faster to consider the group which is smaller, and strip that leader (let's call him B) of his title and update his leader to the larger group's leader (let's call him A).
<br><br>
Now the question arises - what about B's followers? Don't they still believe B is their leader? Their <i>parent</i> array indices still state that their leader is B, right?
<br><br>
Well, this isn't a problem as you'll soon see in our <i>find( ) </i>function's implementation.
<br><br>
But for the join function's implementation, it's as follows -
<br><br>
```c++
int join(int x, int y) {
	if(find(x)>find(y)) 
		swap(x, y);
	par[ find(y) ] = find(x);
}
```
The crux to gather from here is - joining two groups is performed by assigning the parent of one leader to the other leader (who is assigned to whom is circumstantial and depends on the question - I'll link an example down below, a question that I solved recently. Made me feel good about this concept lol.)
<br><br>
That's the essence of the <b>join </b>function.
<br><br>
<hr>
<h2>Find( )</h2>
Now let's get on to the <i>find( )</i> part of union-find algorithms.
<br><br>
This implementation, allows you to find the leader of the group in O(1) on average which is pretty handy because you need to use the find( ) function even to unite two groups.
<br><br>
Personally, I know the drawbacks of the following implementation (it's recursive) but I love the simplicity of it. It's so beautiful to look at.
<br><br>
<i>Okay, sorry, drifting away. Back. </i>
<br><br>
Here is my implementation in C++.
<br><br>
```c++
int find(int x) {
	return par[ x ] = (par[ x ] == x)? x : find(par[ x ]);
}
```
So, let's work through this. What's happening here, is as follows.
<br><br>
My <b>base case</b>, is when the <i>parent </i>array and its index are the same. This is when I'm looking at a leader, or a person who has no leader.
<br><br>
<b>Therefore he is his own leader. </b>And that's how I know I've either reached the top of a group or just one lonely dude.
<br><br>
Now, what if the <i>parent</i> array indicates the parent of my current index is not itself?
<br><br>
That's my <b>general case. </b>This means he HAS to be a follower of somebody. How do I find who he's following? Easy -- <i>parent[current_entity] </i>literally points me to the current entity's parent.
<br><br>
So, now that I know who's the current person's immediate leader, can I say I know this leader is the group's leader? <b>No. </b>Well, at least not with my implementation of my join function.
<br><br>
Why? Because if a group merges with another group, the leader who became a follower of the main leader, still has his followers thinking that the overall leader is the same while a change was made.
<br><br>
So, the <b>recursive</b> nature of the implementation is necessary to identify the overall group's leader.
<br><br>
Hence the recursion, hence the implementation as such.
<br><br>
I suppose that's it? And here's the question I was talking of -
<br><br>
http://codeforces.com/problemset/problem/893/C
<br><br>
And here's my solution - <a href="http://codeforces.com/contest/893/submission/32615359">http://codeforces.com/contest/893/submission/32615359</a>
<br><br>
<i>Well, that's an overview of Disjoint sets, I really like this because usually it's a very convenient substitute for standard graph algorithms like DFS or BFS and it never gives me issues with its efficiency. </i>
<br><br>
<i>So yeah, that's that. Hope it helped. </i>
<br><br>
<i>Aditya</i>