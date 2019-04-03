---
layout: post
title: "Grokking Graph Theory"
excerpt: "Due to popular demand, let's deal with graph theory, from the bottom-up! (DP joke, sorry)"
categories: [algorithm]
image:
    feature: 
comments: true
---

<i>I'll say it -- applying graph theory can be as frustrating as it is satisfying. From DFS/BFS to Dijkstra's, Kruskal's and Prim's algorithms, and more advanced techniques like combining SegTrees/BITs with graphs? Suffice it to say, it gets rough.</i>
            <br><br>
            <i>Anywho, let's get started!</i>


<hr>


<h2>Graphs, the lingo.</h2>

Okay, so to explain graph algorithms, we need to be on the same page and for that, <b>if you already are familiar with graph terminologies, skip this section of the article and jump to DFS</b>, else, read this and I'd also suggest watching <a href="https://www.youtube.com/watch?v=h9wxtqoa1jY">MIT's ~85 minute lecture on Graph Theory.</a>


So! The vocabulary in the field of graph theory -
<br><br>
<b>Node</b> - A point, or <b>a vertex</b>.
<br><br>
<b>Edge</b> - A line connecting two nodes/vertices, representing connectivity or a path between the two nodes.
<br><br>
<b>Directed Edge</b> - An edge which signifies only one-way connectivity, i.e. if A-&gt;B, it is possible to move from A to B but <b>not </b>from B to A.
<br><br>
<b>Undirected Edge</b> - No restrictions on movement exist. It is a two-way street.
<br><br>
<b>Cyclic Graph</b> - A set of nodes connected in such a manner that it is possible to reach the starting node by traversing some (or all) edges just once.
<br><br>
<b>Graph</b> - A collection of vertices and edges.
<br><br>
<b>Tree</b> - A collection of vertices and edges, such that <b>no cycle </b>exists, i.e. An <b>acyclic graph</b>
<br><br>
<b>DAG</b> - Directed Acyclic Graph (A directed tree)
<br><br>
<b>Root </b>- Exclusive to trees, it is the topmost level of the tree. There can only be one root in any tree.
<br><br>
<b>Child/Parent </b>- A node X that can be reached by another node Y only, implies X is the child of Y and Y is the parent of X.
<br><br>
<b>Ancestor </b>- Similar to a parent, but across a larger distance, i.e. to get to X, if I have to go through Y and further levels and only then can I get to X, Y is an ancestor of X.
<br><br>
<hr>
<h2>Storing a graph</h2>
Now, two ways to store a graph as information.
<br><br>
<b>Adjacency Matrix - </b>A 2-D array <i>adj</i>, where <i>a</i><i>dj[i][j]</i> signifies the status of the connectivity of nodes <i>i </i>and <i>j</i>. If <i>adj[i][j] = 0, </i>then <i>i </i>and <i>j</i>, are not connected, and if <i>adj[i][j] = k</i>, they are connected with an edge weight of <i>k.</i>
<br><br>
<b>Adjacency List -</b> Also, a 2-D array, where <i>adj[i] </i>stores a 1-D array of nodes <i>i</i> is connected to. i.e. if <i>j</i> is one of the nodes present in <i>adj[i]</i>, that implies <i>i </i>and <i>j</i> are connected.
<br><br>
<hr>
<br><br>
<h2>Depth First Search</h2>
Alright, so let's get into DFS.
<br><br>
DFS is my first choice, when it comes to "Find a route from A to B", or "Can I get from A to B via C without going through D?" type of questions, or any situations which can be molded into these types of questions.
<br><br>
The idea is simple -- <b>You check ALL possible routes.</b>
<br><br>
That's how DFS works. Check all paths, stop when you find a path satisfying your condition.
<br><br>
Now there's a catch -- <b>If the graph contains a cycle, </b>you'll HAVE to use a hash array to check if a given node has been previously visited or not, to prevent an accidental infinite loop. <b>If the graph is acyclic, </b>you'll need nothing of the kind.
<br><br>
Now, consider the following problem -
<br><br>
<i>You're driving a fancy car. This car has enough petrol to only last 60 km. Check if there's a route from your home to a friend's, and also check if the distance is within 60 km to see if you can drive over to their place, assume its a small city and you know which road leads to whose house. </i>
<br><br>
So now, you'll have to run a depth-first-search taking your house as the root, and in your recursive procedure, the base case would be if the node you're at, is your friends home. Also, passing an extra variable as a parameter that keeps track of current distance travelled, will help you check if you can reach their home without needing a petrol refill.
<br><br>
The pseudocode would be as follows -
            
```c++
int dfs(int node, int destination, int distance):
   if node equals destination and distance is within 60:
     print "Friend is reachable"
     return
   else:
     for all neighbours of node:
        dfs(neighbour, destination, distance + edgeWeight[node][neighbour])

```
And that's the basics of depth first search. The recursive calling stops if the base case has been reached OR if all nodes have been visited, whichever happens first.
<br><br>
            Now, on to the next topic.
<br><br>
<hr />
<h2>Breadth First Search</h2>
Similar to DFS, the difference being, in DFS, paths are traversed depth-wise, (choose one path, keep going till you reach the end) but in a BFS, there are no "paths", there are areas. As in, at every step you process information equidistant from the start (root) node.
<br><br>
When thinking of BFS, I always picture something like the ripples of water, or a nuclear explosion. The circle of effect spreading? <b>That's </b>the breadth first search. Here's an example of what kind of problem a technique like this can solve -
<br><br>
<i>Given an NxN grid consisting of either 0s,1s or 2s, the following nomenclature exists - </i>
<br><br>
<i>0 - an empty tile </i>
<br><br>
<i>1 - the location of a person </i>
<br><br>
<i>2 - the location of an alarm </i>
<br><br>
<i>Now, assume all people have their mobile phones and whenever the first person hears an alarm ring, they instantly alert everyone else through a text message. </i>
<br><br>
<i>The time a person at (x, y) takes to hear an alarm at (a, b) is equal to |x-a|+|y-b| </i>
<br><br>
<i>Find how long it takes for all people to be alerted if the building catches fire. </i>
<br><br>
Now, this question serves as a nice example where BFS can be applied. The idea is to perform a BFS on all alarms and find the time to alert the closest person to each alarm. Find the global minimum of these times and you have your answer.
<br><br>
Which leads us to the next question, how would you implement this, and more generally, how would you implement BFS?
<br><br>
Well, I prefer the iterative implementation of BFS as I find it more intuitive to understand. In a BFS, we generally use <b>a queue. </b>The way it works is as follows -
<br><br>
So in the above question, you'd have to call BFS on each alarm, and upon finding a person, calculating the time taken to reach this person and changing the final answer accordingly.
<br><br>
That'd go as follows -
            
```c++
int bfs(int start):
    create an empty queue
    push in start
    while the queue is not empty:
        temp = queue.front()
        queue.pop_front()
        if temp is a person:
            current_time=time taken for alarm to reach temp
            minimum_time=min(minimum_time, current_time)
            break
        for all neighbours in temp:
            queue.push_back(neighbour)
    return minimum_time
```
    
Calling the above method over every alarm, will provide you with the minimum time each alarm takes to alert the closest person. Find the minimum of these and <i>voila!</i>


<br><br>
And that's BFS for you!
<br><br>
<hr />
<h2>Dijsktra's Algorithm</h2>

<br><br>
Dijkstra's Algorithm, is used to determine the shortest path from a given source to a destination. It's complexity is O(V²) [and can be optimized to O(E * logV) ]and the crux of it is -
<br><br>
<b>What's the least distance I must travel to reach node X </b><b><span style="text-decoration:underline;">through</span></b><b> all neighbours of X, if I know the least distance I must travel to visit X's neighbours.</b>
<br><br>
Logically, to get to X from another node <i>(say 'Y')</i>, REQUIRES you to travel through atleast one of X's neighbours. So if you could calculate the distance you must travel to reach all its neighbours, you can then make an educated decision on which route is the shortest of all routes. It's similar to dynamic programming, as you're solving subproblems, i.e. solving the distance from current node to transit node, and then transit node to destination node.
<br><br>
Obviously, to store the results of all these subproblems, you'll need to create an array <i>dist </i>of size <i>V, </i>to where <i>dist</i><i>[i]</i> stores the distance from the current node (for whom the array was made) to the node <i>i</i>. Therefore by the end of all the iterations, the array <i>dist</i>, stores the distances from the current node to every other node.
<br><br>
So effectively, you iterate over every node, and for each node you re-iterate over every node to see if there's a new shorter possible route to your outer node through your inner node.
<br><br>

The pseudo code flows as -
            
```c++
dist[]={inf, inf, inf, inf, ...}
for all nodes (i):   
   for all nodes (j): 
     if i and j are connected: 
        dist[j]=min(dist[j], dist[i]+len[i][j]
```
<hr>
<h2>Floyd Warshall's Algorithm</h2>
The ASSP (all source single point) algorithm that runs in O(V^3) and calculates the shortest distance between all pairs of points.
<br><br>
I prefer to understand it instead as just running dijkstra's over all vertices, and in fact the original algorithm can be optimized by using an adjacency list instead of a matrix.
<br><br>
The pseudo code flows as follows -
<br><br>


```c++

    ans[][]=[[inf, inf, inf, ...], [inf, inf, inf, ...], ... ]

    for all nodes (k): //intermediate node
        for all nodes (i): //start node
            for all nodes (j): //end node
                ans[i][j]=min(ans[i][j],ans[i][k]+ans[k][j])
            //NOTE: LITERALLY removing the '[i]' from the prev line gives Dijkstra's
            //That is how similar they are.

```
I'll be frank, I've never needed to use Floyd Warshall's algorithm so far simply because having Dijkstra's algorithm in my arsenal was good enough.


<hr>

<h2>Minimum Spanning Trees</h2>

Firstly, a spanning tree is a tree that connects every node in a graph. A <b>minimum</b> spanning tree, is a spanning tree whose sum of edges is the least, across all other spanning trees.

<img src="http://i1.wp.com/blog.hackerearth.com/wp-content/uploads/2017/01/Minimum-spanning-tree-1.png?resize=730%2C328" />


Now, there are two famous MST algorithms. Prim's algorithm, and Kruskal's algorithm. I personally prefer Kruskal's algorithm since I find it relatively easiest to understand, prove and implement, not to mention both algorithms have the same average time complexity O(ElogV) so the difference further reduces.

<h2>Kruskal's Algorithm</h2>

<strong>Pre-requisite: Disjoint set union data structure </strong>



Kruskal's algorithm works by sorting edges by their values in ascending order, and picking the vertices accordingly as part of the MST. This makes sense intuitively as the smallest available global edge at any point of time must be a part of the MST, as the same tree would be the MST whether that edge existed or not. i.e., if you're going to connect a node to an MST, it's optimal to connect it with the smallest edge attached to it.
<br><br>
This is the proof of why Kruskal's algorithm can find the MST --
<br><br>
Assume you have connected a node <i>n</i> to your MST with an edge whose value is greater, than the least value of the edge connected to <em>n</em>. Now let the length of the MST (sum of all edges of this tree) equal <em>k. </em>Now, <em>k </em>must be the minimum value of the lengths of all spanning trees (for it to be a MINIMUM spanning tree), but now if you instead considered the edge connected to <em>n </em>which had the least value, it is obvious that the new tree's length is lesser than the previous one, implying that we must always pick the smallest edge connected to a node. Thus, Kruskal's algorithm is proved.
<br><br>
The <strong>pseudo-code</strong> for this also requires the implementation of the disjoint set data structure and <strong>will therefore be uploaded on my GitHub profile</strong>, which I'll link to here shortly.
<br><br>
<hr />
<br><br>
So yeah! That's the start of graph theory! The next article on graph theory would be mixing it with segTrees/BITs, and to prepare yourself for that, consider reading up on SegTrees. <a href="/2017/08/24/traversing-segment-trees.html">You can do that here! - Traversing Segment Trees</a>.
<br><br>
<hr />
<br><br>
<em>Phew! Long article. Almost 2,000 words. K. I'm gonna sleep. Ciao.</em>
<br><br>
<em>Aditya</em>