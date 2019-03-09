---
layout: post
title: "[Topcoder Weekly Challenge] Google Kickstart Practice Round!"
excerpt: "Solving Google Kickstart's Practice Round's Final Problem."
categories: [algorithm, topcoder]
image:
    feature: 
comments: true
---

> This article was originally published by me on Topcoder at: [https://www.topcoder.com/blog/problem-of-the-week-google-kickstart-alarm/](https://www.topcoder.com/blog/problem-of-the-week-google-kickstart-alarm/)

Google recently launched the 2019 edition of their yearly Kick Start algorithmic contest series. 

On February 24th, Google hosted a practice round as a warmup and this article will be based on analyzing and approaching this practice round’s third (and final) problem statement – [Kickstart Alarm](https://codingcompetitions.withgoogle.com/kickstart/round/0000000000051060/0000000000058a56) 

## The problem statement
You possess an alarm clock that rings K times. The ith alarm ring (1<=i<=K) has a loudness equal to Power(i).

Given an array A of N elements, Power(i) is defined as the “ith exponential-power” sum over all subarrays of A.

The i-th exponential-power of subarray Aj, Aj+1, …, Ak is defined as Aj × 1^i + Aj+1 × 2^i + Aj+2 × 3^i + … + Ak × (k-j+1)^i. 

Thus, summing this up over all subarrays of A (for all pairs of j, k where 1<=j<=k<=N) gives us Power(i)

The problem statement now asks us to calculate the value of –

Power(1)+Power(2)+Power(3)+….+Power(K) 

That is, the total power of all the K alarm clock rings. Output the result modulo 10^9+7. 

## The approach

The problem statement may be slightly convoluted, but the next paragraph that explains the brute force approach should help formalize the problem statement. 

The simplest approach to the problem would be to iterate for all powers (O(K)) and for each power level, fix a left end of a subarray (O(N)), fix a right end of a subarray (O(N)), and iterate over the fixed subarray at the current power level (O(N)) and calculate the contribution of this subarray at this power level and add it to the final answer. 

This has a total time complexity of O(K*(N^3)) which won’t be able to compute the answer to a large dataset efficiently. 

So how do we optimize our solution? By leveraging math and some intuition. 

As the problem statement states, our final result is the sum of the K alarm rings. 

Also, these power rings are defined individually as the sum of ith exponential powers over all subarrays. 

And the ith exponential power of a subarray is the sum of each element multiplied by their index in the subarray raised to the current power level of the alarm. 

As convoluted as all this can seem, the underlying point of interest is that ALL of these terms are only interacting with each other under the operation of addition. 

And since addition is associative, we can rearrange some terms. 

But what do we rearrange? Well, let’s ask ourselves another question – can we compute the “contribution” to the final answer, by each individual element? 

If we can, then the sum of these answers for each individual element must equal the answer of the entire array, right? 

This is a valid approach here, because the answer of a specific element is independent of the answer of any other element as each term in my equation of Power(i) contains only one array element at a time, excluding numerical coefficients. 

Thus, we CAN isolate each element, determine the total power it contributes over all K alarm rings by counting which subarrays it could be present in, and at what possible indexes, and once we compute this for all elements of the given array, we can add up all their results to obtain our final answer. 

This little observation above is crucial to solving the problem, so it’s important to fully process this, before moving on. 

So how do we count each element’s contribution? 

Consider an element that occurs in the Jth index of the initial array A. If we can compute the contribution for this element, then we can simply iterate over all values of J from 1 to N. 

So, let’s consider an element in the Jth index of the initial array A. 

This element can be the first element in (N-J+1) subarrays.  (All subarrays containing this element as the leftmost element)

Similarly, this Jth element can be the second element in (N-J+1) subarrays by fixing the (J-1)th element to be the first element of a subarray. 

Similarly, this Jth element can be the third element in (N-J+1) subarrays by fixing the (J-2)th element to be the first element of a subarray. 

This can be extrapolated further, and we can now compute the contribution of the Jth element at Power level i, since we have the frequency with which this element occurs. 

Now, if we look at the Jth element across all power levels, we see that the power levels form a geometric progression. 

And since the problem statement asks us to find the sum of all K powers over all these elements, we can compute the same by instead determining the sum of all these elements over K powers separately, and then adding the results up. 

And that’s exactly what the top scorers of the round did. 

Here is the accepted solution of lee3000, who ranked 1st in the contest. (Note the implementation below keeps 1-based indexing in mind)

```c++
#include <bits/stdc++.h>
#include <vector>
#include <set>
#include <map>
#include <string>
#include <cstdio>
#include <cstdlib>
#include <climits>
#include <utility>
#include <algorithm>
#include <cmath>
#include <queue>
#include <stack>
#include <iomanip> 

using namespace std;
long long a[1000001];
long long mod=1000000007;
long long px[1000001];
int n,k;
inline long long power(long long x,long long y)
{
    long long t=1;
    while(y!=0)
    {
        if(y%2==1)
            t=t*x%mod;
        x=x*x%mod;
        y/=2;
    }
    return t;
}
inline void prepare()
{
    int i;
    for(i=1;i<=k;i++)
    {
        px[i]=px[i-1]+power(i,i);
        px[i]%=mod;
    }
}
int main()
{
//    freopen("C-small-attempt0.in","r",stdin);
//    freopen("C-small-attempt0.out","w",stdout);
    // freopen("C-large.in","r",stdin);
    // freopen("C-large.out","w",stdout);
    int T,kk=0;
    scanf("%d",&T);
    while(T>0)
    {
        kk++;
        T--;
        scanf("%d%d",&n,&k);
        long long x1,y1,c,d,e1,e2,f;
        scanf("%lld%lld%lld%lld%lld%lld%lld",&x1,&y1,&c,&d,&e1,&e2,&f);
        int i,j;
        long long x,y;
        a[1]=(x1+y1)%f;
        for(i=2;i<=n;i++)
        {
            x=(c*x1+d*y1+e1)%f;
            y=(d*x1+c*y1+e2)%f;
            a[i]=(x+y)%f;
            x1=x;
            y1=y;
        }
        long long ans=0;
        long long la=k;
        for(i=2;i<=n+1;i++)
        {
            ans=(ans+la*(long long)(n+2-i)%mod*a[i-1]%mod)%mod;
            long long x=(power(i,k+1)-1)*power(i-1,mod-2)%mod;
            x--;
            if(x<0)
                x+=mod;
            la+=x;
            la%=mod;
        }
        printf("Case #%d: %d\n",kk,ans);
    }
    return 0;
}

```


--------------------------

Well! I hope this added some value to you! Feel free to let me know your comments. :)

For more of my work, follow me on:

LinkedIn: [https://www.linkedin.com/in/adityaramesh1998/](https://www.linkedin.com/in/adityaramesh1998/)

GitHub: [https://github.com/RameshAditya/](https://github.com/RameshAditya/)

Twitter: [https://twitter.com/adityaramesh98](https://twitter.com/adityaramesh98)

Cheers!