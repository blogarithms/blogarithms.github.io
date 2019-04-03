---
layout: post
title: "Exploring Binary Search"
excerpt: "Identify a change in state of a bivalued function! This goes beyond the 'find-in-array' idea."
categories: [algorithm]
image:
  feature: 
comments: true
---
I first learnt binary search back in 12th grade. At first it seemed like a nifty, little algorithm that only looked fancy and sounded like a bigger deal than what it deserved to be. Boy, was I wrong. <em>(shocking, I agree)</em>
            <br><br>
            Looking back, I also remember thinking I was a good chef at the time so my judgment may have been a tad bit faulty.
            <br><br>
            In short, the common example given to most of us is -
            <br><br>
            <em>Find if an element exists in a sorted array of n elements</em>
            <br><br>
            And binary search accomplishes this in an impressive O(logn), as it halves its search space at every iteration, by comparing the value of the element to be found, with the middle element in the search space. But in the recent months, I've learnt that there's more to binary search than meets the eye.
            <br><br>
            Of course, a pre-requisite of implementing binary search is that the array must be sorted.
            <br><br>
            This pre-requisite is, frankly, not the most general pre-requisite. But we're getting ahead of ourselves. First, the simple, elementary binary search implementation -
```c++
    int bSearch(int arr[], int l, int r, int x){
               if(r >= l){
                    //Initialize mid in this manner to avoid integer overflow as compared to (l+r)
                    int mid=l+(r-l)/2;

 	//If the element is equal to the mid element
 	if(arr[mid]==x) return mid;
 
   //If element is smaller than mid, then it is present in the left half
   if(arr[mid]>x) return binarySearch(arr,l,mid-1,x);

   //Else the element is present in right half
   return bSearch(arr,mid+1,r,x);
}
   //Element not in array
   return -1;
}
```
The above code is an implementation of trying to find an element in a sorted array.
<br><br>
And this is where 12th grade me thought the fun ended. Lucky for us, the star attraction is just starting.
<br><br>
This is where we start getting fancy. Remember that "pre-requisite" I was going on about? Here it comes.
<br><br>
<strong>I'll explain with an example.</strong>
<br><br>
<em>Consider this scenario. Sherlock Holmes was told by "Jack" that he saw a man dead at 6.00 pm Friday. Jack also tells him that he last saw this man on Monday at 1.00 pm. Sherlock needs to find when the man died.</em>
<br><br>
<em>Sherlock's search space is obviously [Monday 1.00 pm - Saturday 6.00 pm]</em>
<br><br>
<em>Now the first close friend of the victim Sherlock asks says he last saw the victim alive on wednesday at 4.00 pm. Call this <strong>interaction 1</strong>.</em>
<br><br>
<em>Sherlock's search space has reduced to [Wednesday 4.00 pm to Saturday 6.00 pm]</em>
<br><br>
<em>Next Sherlock asks his wife and she says the last time she saw him was on Friday 5.00 pm. <strong>Interaction 2</strong>.</em>
<br><br>
<em>New search space, [Friday 5.00 pm to Saturday 6.00 pm]</em>
<br><br>
<em>The victim's butler now finds Sherlock and tells him that he saw the victim dead at Friday 6.00 pm, but was too afraid to tell anybody. <strong>Interaction 3</strong>.</em>
<br><br>
<em>The search space is now [Friday 5.00 pm to Friday 6.00 pm]</em>
<br><br>
<em>Sherlock is content and identifies the murderer with this information. </em>
<br><br>
Now, did you see it? Did you see binary search in action?
<br><br>
<strong>General pre-requisite: Binary search can be implemented to solve for the first occurrence (or last occurrence) of a certain value in a strictly bi-valued function where f(x) implies f(y) for all y>x. For example, [0, 0, 1, 1, 1, 1] or [alive, alive, alive, dead, dead]</strong>
<br><br>
What that means in this example is, Binary search can be implemented to solve for the first hour of dead state (or last hour of alive state) of the status of the victim in a function "What was the victim's state when you last saw him?" [Dead (0) or Alive (1)].
<br><br>
This is the generalization of binary search. Abstracting it from finding a value in an array, to finding a valid value <strong>in a function</strong>. After all, in the case of finding a value in an array, all you're doing is finding a valid value in a function whose input-output mapping is given by indexing (x: arr[x]) as compared to a general equation. (x: func(x))
<br><br>
Additionally, keep in mind when adjusting lower and higher bounds to check whether the mid value must be included or excluded in the search space. This is crucial, and an off-by-one error could lead to an infinite loop, or worse, a wrong answer.
<br><br>
An example of this abstracted form of binary search can be found in this question -- <a href="http://www.spoj.com/problems/AGGRCOW/">SPOJ - AGGRCOW</a>
<br><br>
So, the pseudo code changes. It now becomes -

```c++
int f(int x){
       return function[x];
}
int lo=0;
int hi=INT_MAX;
while(lo&lt;hi){
        int mid=lo+(hi-lo)/2;
        if(f(mid))lo=mid;
        else hi=mid-1;
}
return lo;
```

So, that's binary search on bi-valued uniform functions.
<br><br>
They can also be used on mathematical questions and they can be done so EXTREMELY efficiently as each iteration halves its search space, binary search can compute over insanely large search spaces very quickly.
<br><br>
The sole difference between implementing it mathematically and otherwise, is that from a mathematical viewpoint, there is no need for a 'lo' and 'hi' pointer for the search space.
<br><br>
200-300 iterations would get an accurate answer instead, <em>(Power of binary search, once again)</em>
<br><br>
All you need, is to be able to come up with the function. If you can come up with one, binary search will do the rest.
<br><br>
I remember an interesting question, the first one which exposed me to abstracting binary search at this level, and it went like the following -
<br><br>
<em>Jake writes a number on paper, and repeatedly divides it by 10, and adds the integer part to the number on the paper. He keeps doing this till the inital number becomes 0. Now, let the final sum be X. Given X, calculate the initial number Jake was given.</em>
<br><br>
<em>For example:
</em><em>Input: X=561
</em><em>Output: 506
Consider 506. Jake divides it by 10 and adds it to the total.
-
Total = 506 + (50)
Number=50
-
Total = 556 + (5)
Number = 5
-
Total = 561 + 0
Number = 0
-
Thus, starting with 506 gives the end result of 561, and therefore if X=561, Jake must have started off with 506.</em>
<br><br>
And that wraps up this post on binary search! Feel free to reach out to me with any comments. :)
<br><br>
<em>Aditya Ramesh</em>