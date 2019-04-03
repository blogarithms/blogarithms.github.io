---
layout: post
title: "Scrutinizing Sieving Algorithms"
excerpt: "Implement the mathematical sieve of eratosthenes to identify the primality of numbers in O(nlognloglogn)!"
categories: [algorithm]
image:
     feature:
comments: true
---
<i>Ah, sieving. This is a stroll down memory lane for me, as nearly 10 months ago, the Sieve of Eratosthenes was the first algorithm I had to learn myself, while attempting to solve a question on SPOJ. But anyways, let's jump right in.</i>

<hr>

So! Sieving!
<br><br>
I discovered this approach on a question, and here's that question -
<br><br>
<span style="color:#ff0000;">Given <i>N</i> queries, each containing a single integer <i>X</i>, determine if the given <i>X</i>, is a prime number or not. If <i>X</i> is prime, print "yes" else print "no".</span>
<br><br>
Now, of course, the question is simple to brute force. The brute force complexity would be O(N*max(X)), or with some pruning and observation-based optimizations, one could speed it up to O(N*√(max(X))), but that wasn't enough. In fact, that was my first solution which, upon showing me a "Time Limit Exceeded" verdict, forced me to resort to reference websites.
<br><br>
Well, the short answer to how you solve the above question optimally, is <b>sieving</b>.
<br><br>
Here's the core principle behind the algorithm -
<br><br>
<b>Given a number </b><i><b>x </b></i><b>(either prime or composite) &gt; 1, we know all it's multiples of the form </b><i><b>k*x </b></i><b>CANNOT be prime. i.e, </b>
<br><br>
<b>∀ x,k ∈ ℕ: k*x ∉ P (where P is the set of all primes)</b>
<br><br>
Thus, here's the way it works in code -
<br><br>
<pre>bool isprime[N]; <span style="color:#0000ff;">//Define a hash map isprime[i] to state if <i>i</i> is prime or not</span>
for(int i=0;i&lt;N;i++)
     isprime[i]=1; <span style="color:#0000ff;">//Assert the case all numbers are prime for now</span>
isprime[0]=0;
isprime[1]=0;  <span style="color:#0000ff;">//Handle base cases</span>
for(int i=2;i&lt;=sqrt(N);i++)
     if(isprime[i]) <span style="color:#0000ff;">//If the current number is a prime, proceed</span>
          for(int j=i*i;j&lt;N;j+=i)
               isprime[j]=0; <span style="color:#0000ff;">//mark all multiples of <i>i </i>as composite</span>

<span style="color:#0000ff;">//That's it! isprime[i] tells us the primality of <i>i </i>now in O(1)!</span></pre>
<br><br>
And that's how a simple prime generator is implemented in the form of a sieve. Now, its possible to check if a number is prime in O(1).
<br><br>
It's worth mentioning that the average complexity of this algorithm is O(N * logN) <i>(More specifically, it's O(N * logN * loglogN) but that last term doesn't matter all that much in practice)</i>
<br><br>
So that's how primality is dealt with, sieve-style! Now let's look at it under different lights.
<br><br>
Sieving is a concept easy to understand but hard to master, every now and then, I still manage to come across a problem where upon reading the editorial, I raise an eyebrow going "<i>Wut, sieving here? Man."</i>
<br><br>
A few nice questions on sieving are as follows -
<br><br>
<hr>

<a href="http://www.spoj.com/problems/PRIME1/">SPOJ - PRIME1</a>
<br><br>
The straight-forward question that asks you to literally apply what you just read above, this one should be a good indicator of how well you picked up this concept.
<br><br>
<hr>

<a href="https://www.codechef.com/problems/KPRIME">CodeChef - KPRIME</a>
<br><br>
In this question, the tl:dr; version of the algorithm is as follows - generate all primes using the above sieving technique into a boolean array <i>isPrime</i>.
<br><br>
After doing this, use a similar sieving algorithm on another array <i>numberOfPrimeFactors</i>, i.e. for every number that is a prime, add 1 to all its multiples for it contributes to them as a prime factor.
<br><br>
And that's all, an AC awaits you.
<br><br>
<hr>

In addition, it's also useful in finding, say, the smallest prime factor of a given number. Consider the following implementation -
<br><br>
<pre>bool isprime[N];
int len, smallest[N];
isprime[0]=isprime[1]=0;
for(int i=2;i&lt;N;i+=2){ 
     isprime[i]=1;
     smallest[i]=2; <span style="color:#0000ff;">//the smallest prime factor of even numbers is 2</span>
<span style="color:#000000;">}</span>
for(int i=3;i&lt;N;i+=2){ 
     if(isprime[i]){ <span style="color:#0000ff;">//if the number is prime</span>
          smallest[i] = i; <span style="color:#0000ff;">//set it to itself</span>
          for(int j=i;(j*i)&lt;N;j+=2){  <span style="color:#0000ff;">//iterate over its odd multiples</span>       
               if (isprime[j*i]) <span style="color:#0000ff;">//if it is prime, reset it, and update smallest prime factor</span>
                    isprime[j*i] = false, smallest[j*i] = i; 
          } 
     }
}</pre>
<br><br>
Of course, there are other ways to determine the primality of a number (Fermat's Little Theorem, Miller-Rabin Algorithm, or the smallest factors of a number, but sieves accomplish them well enough too and are easy to implement quickly.
<br><br>
There's also an O(N) implementation of a prime sieve, and that's published in an ACM journal. Here's the link to the research paper - <a href="http://www.cs.utexas.edu/users/misra/scannedPdf.dir/linearSieve.pdf">http://www.cs.utexas.edu/users/misra/scannedPdf.dir/linearSieve.pdf</a>
<br><br>
And that's the basics of sieving!
<br><br>
Good luck applying it!
<br><br>
<i>Aditya</i> 