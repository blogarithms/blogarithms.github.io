---
layout: post
title: "Learning Lazy Propagation"
excerpt: "Perform Range Updates In Logarithmic Time With This Data Structure! Pre-Requisite: Segment Trees"
categories: [algorithm]
comments: true
---
Today's topic is <b>lazy propagation</b>.
            <br><br>

I highly recommend you brush up on Segment Trees before reading this article as lazy propagation is an algorithm that's implemented on segTrees.

Now that that's done with, let's jump in.
<br><br>
<b>Lazy Propagation is a range-update optimization implemented on segTrees that performs range updates in O(logn) time.</b>
<br><br>
It works by being lazy. <i>(surprise, surprise.)</i>
What that means, is that it only updates nodes when necessary, i.e. when a query is being performed. This means that if 5 update operations were performed and then 1 query operation, the actual values of the segTree are updated only during the 6th operation.
<br><br>
Consider this question: <i>You are given an array A with N elements, and Q queries follow.</i>
<i>Each query is of 2 types. "1 L R" where you're required to find the minimum element from L index to R index, and "2 L R X" where you're required to update all values in A from L index to R index by adding X to them."</i>
<br><br>
Now, query 1 is solvable by a standard segTree, in fact it's the standard RMQ problem, and the article on segTrees on my blog should suffice in teaching how to solve & implement it.
<br><br>
But solving query 2 optimally, requires something else. A point-update implementation isn't good enough. It's worst-case time complexity is O(n * logn)
<br><br>
We need something better. We need <b>lazy propagation</b>.
<br><br>
Lazy propagation updates ranges in a complexity of O(logn), orders of magnitudes better than the point-update method.
<br><br>
So how does it work?
<br><br>
<b>The Crux:</b>
It's implemented as an array and must be visualized identical to the segTree. <b>Each node on the lazy propagation </b><b>tree </b><i><b>(here on referred to as 'lazy node'),</b></i><b> stores the update to be done on the segTree's node. </b><b>During a query, the lazy nodes are also checked, and the segTree's nodes are updated if needed and returned accordingly.</b>
<br><br>
Now, a little more into the details. A lazy tree is created, and <b>visualized identical to the segment tree</b>. Each index refers to the corresponding node on the lazy tree as it does on the segment tree. (1-based indexing, for convenience)
<br><br>
Index 1 corresponds to the root of the segTree and the root of the lazy tree.
<br><br>
Indexes n to (2 * n)-1 correspond to the leaves of the segTree and the leaves of the lazy tree.
<br><br>
Now, let's get to the implementation.
<br><br>
<b>Building the lazy tree</b>
<br><br>
We'll consider the above RMQ question. So our range-update operation requires us to add a given value X to a given range [L,R]. Now, the most useful information our lazy node can hold, is the value of X. i.e. the value of a lazy node will tell us how much to add to our segTree's node.
<br><br>
Therefore, the initial state of the lazy tree's elements will all be equal to zero.
<pre>for(int i=0;i<4 * N; i++) lazy[i]=0;</pre>Now, handling queries of type 2. (update queries)
<br><br>
<b>Updating the lazy tree</b>
<br><br>
Updating the lazy tree is done, very similarly as querying in a normal segTree, as your objective is the same in both -- Find the most appropriate node/nodes to look at.
<br><br>
Once you've found this node, the difference now is, you'll be adding X to the value at the lazy node. Therefore, the implementation would look like -
```c++
int range-mark(int node, int start, int end, int l, int r, int val){
        //no overlap with node
        if(r<start||l>end)return 0;

        //total overlap with node
        if(l<=start && end<=r)lazy[node]+=val;

         //partial overlap with node
         else{
                int mid=(start+end)>>1;
                range-mark(node<<1,start,mid,l,r,val);
                range-mark(node<<1|1,mid+1,end,l,r,val);
            }
}
```
Now, handling queries of type 1.
<br><br>
<b>Querying on the segTree with the lazy tree</b>
<br><br>
Once again, querying on a segTree isn't any different from the normal implementation. The only change here, is that the value of the segTree's node will have to be updated after checking the value of the corresponding lazy node.
<br><br>
Also, if the lazy node indicates an update is pending i.e. it's value is non-zero, perform the update on the segTree's node, mark the children of the lazy node with the value of their parent's (current lazy node), and then reset the current lazy node's value back to zero.
<br><br>
Thus, the pending update has percolated to the next layer in O(1) only when necessary and the query's value is received in the time it takes to travel the depth of the tree.
<br><br>
It's implementation would be as follows -
```c++
int query(int node, int start, int end, int l,int r){
        //No overlap, useless node
        if(start>r||end<l)return 0;

        //Clear lazy node, pass on the lazy node's value to
        //it's children, update segTree's node
        if(lazy[node]!=0){
                segTree[node]+=lazy[node];
                lazy[node<<1]=lazy[node];
                lazy[node<<1|1]=lazy[node];
                lazy[node]=0;
         }
         //Total overlap
         if(l<=start && end<=r)return segTree[node];

         //Partial overlap
         else{
                int mid=(start+end)>>1;
                int a=query(node<<1,start,mid,l,r);
                int b=query(node<<1|1,mid+1,end,l,r);
                return min(a,b);
         }
}
```
And that is how lazy propagation is used in range-update operations.
<br><br>
A nice question I solved using this approach, is this one - <a href="https://www.codechef.com/problems/EQUAKE" target="_blank" rel="noopener">CodeChef - Earthquake</a>
<br><br>
I solved it with some pre-processing (for all left rotations modulo string length), and segTrees + lazy propagation. My nodes held the value of the left shifts needed modulo the string length.
<br><br>
And that brings me to the end of this article. I hope you enjoyed it! :)
<br><br>
<i>Aditya Ramesh</i>