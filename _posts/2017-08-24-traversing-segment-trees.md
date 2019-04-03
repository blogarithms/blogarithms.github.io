---
layout: post
title: "Traversing Segment Trees"
excerpt: "Learn How To Perform Range Queries In Logarithmic Time With This Data Structure!"
image:
  feature: 
categories: [algorithm]
comments: true
---
Alright! So, in competitive programming, a powerful data structure exists, called Segment Tree (SegTree)
            <br><br>
            It's used primarily to optimize range queries from O(n) to O(logn) where 'n' refers to the number of elements.
            <br><br>
            Consider this example -
            <br><br>
            <em>You are given an array A of N numbers. You are also given Q queries. Each query is of the form "L R" (without quotes) and you are required to return the sum of the values from index L to index R.</em>
            <br><br>
            Now, the brute force technique is a matter of implementation. But a segment tree can also be used here. Another common problem is the RMQ problem (Range Minimum Query) where you're required to find the minimum value between a given L and R index.
            <br><br>
            Okay, so now the motivation to learn it is clear. Segment Trees are binary trees, as the name implies, and the special property of a segment tree, what makes it so powerful is -
            <br><br>
            <span style="text-decoration:underline;">Each node stores the result of its children</span>
            <br><br>
            In one line, that's it. That's the big deal.
            <br><br>
            Let's solve the range-sum query problem (given above in italics)
            <br><br>
            <strong>Here, the leaf nodes of the segTree will be the corresponding values in the given array. </strong>
            <br><br>
            Now, the recursive implementation of segTrees requires 4*N space. Its construction takes O(n*logn) as each element is accessed once and for each element, the depth of the tree is traversed.
            <br><br>
            Consider the array A = {5, 3, 1, 8, 6, 7, 21, 9}
            <br><br>
            <img class="alignnone size-full wp-image-86" src="https://rameshaditya.files.wordpress.com/2017/08/st.png" alt="ST" width="772" height="360" />
            <br><br>
            In the diagram, the green numbers are the values on the segTree, the blue numbers are the indices of the segTree (For convenience's sake, use 1-based indexing as the children of node x are 2*x and 2*x+1), the red hyphenated numbers represent the range of the pre-processed solution (referred to as <strong>node range</strong>)
            <br><br>
            Therefore, building this would be the following way -
```c++
    int build(int node,int start,int end){
        if(start==end)tree[node]=A[start];
            else{
                int mid=(start+end)>>1;
                build(node<<1,start,mid);
                build(node<<1|1,mid+1,end);
                tree[node]=tree[node<<1]+tree[node<<1|1];
              }
}
```

Once you've understood that, read this next bit.
<br><br>
The way the segTree works is that once you're given a query, it repeatedly splits the query into ranges it may have preprocessed for, and once the <strong>node range is a subset of the query range</strong>, it returns the value at that node. Else it again branches down into both children.
<br><br>
So let us consider three types of overlap:
<ul>
  <li>No overlap</li>
  <li>Total overlap</li>
  <li>Partial overlap</li>
</ul>
<span style="text-decoration:underline;">No overlap</span> - The node range is outside the query range. Thus, you're looking at a useless node here. Nothing to do, move on.
<br><br>
<span style="text-decoration:underline;">Total overlap</span> - The node range is entirely inside the query range. This is a completely valid node and a very useful one. Return the value in this node.
<br><br>
<span style="text-decoration:underline;">Partial overlap</span> - A part of this node range belongs the query range, therefore recurse on both children until there is "no overlap" or "total overlap".
<br><br>
That's the query algorithm.
<br><br>
It's implementation would go as follows -
<br><br>
(Note: <em>start </em>and <em>end </em>here refer to the node ranges, while <em>l </em>and <em>r</em> correspond to the query range.)
            
```c++
int query(int node,int start,int end,int l,int r){
 //No overlap  
  if(l>end || r<start)return 0;
 //Total overlap
  if(l<=start && end<=r)
   return tree[node];         
 //Partial overlap      
  else{         
   int mid=(start+end)>>1;
   int a=query(node<<1,start,mid,l,r);
   int b=query(node<<1|1,mid+1,end,l,r);
   return a+b;
  }
 } 

```

  and that is how the query algorithm is implemented.
  <br><br>
&nbsp;
<br><br>
One of my favorite questions on SegTrees is - <a href="https://www.codechef.com/MAY17/problems/CHEFSUBA" target="_blank" rel="noopener">Chef and Sub Array</a>
<br><br>
A brute force would fetch 30 points, but a SegTree will fetch 100 points.
<br><br>
Now consider an addition to the question. Assume we were also required to <strong>update the value of an index '<em>idx' </em>of A by a value '<em>val'.</em></strong><em> </em>Then, you'd have to implement a point-update method.
<br><br>
This point-update method would be very similar to the build method. The primary difference being -- the partial overlap case would have to be modified to invoke the recursive function only if <em>idx</em> lies between <em>start</em> and <em>end </em>(as you're trying to find that index to update), and when the base case is reached, <em>A[idx]+=val; </em>and <em>tree[node]+=val; </em>
<br><br>
Due to the recursive nature of the function, all parents of the updated node will also update themselves.
<br><br>
It's implementation would be as follows -

```c++
int update(int node, int start, int end, int val, int idx){
        if(start==end){
                tree[node]+=val;
                A[idx]+=val;
         }
         else{
                int mid=(start+end)>>1;
                if(start<=idx && idx<=mid)
                        update(node<<1,start,mid,val,idx);
                else
                        update(node<<1|1,mid+1,end,val,idx);
                tree[node]=tree[2*node]+tree[2*node+1];
         }
}
```
Update complexity is also O(logn) as the depth of the tree has to be traversed.
<br><br>
However if a range-update is needed, the point-update method's worst case complexity devolves to O(n * logn), which must be solved using another technique termed <strong>"Lazy Propagation"</strong>, which you will find explained in-depth <a href="https://rameshaditya.wordpress.com/2017/09/01/learning-lazy-propagation/">here</a>. :)
<br><br>
In my experience, segment trees are a potential solution when range queries-updates need to be performed optimally, and even more so when a function is commutative and associative. Anywho, I hope this tutorial on segment trees was educational. Feel free to ping me with any comments!
<br><br>
<em>Aditya Ramesh</em>