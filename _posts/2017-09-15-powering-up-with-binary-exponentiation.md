---
layout: post
title: "Powering Up With Binary Exponentiation"
excerpt: "A quick and clever algorithm to exponentiate in logarithmic order of the exponent!"
categories: [algorithm]
image:
    feature: 
comments: true
---
<em>Off-topic, this is quite the hectic time for me. Between course projects, passion projects, make-a-thons, online programming contests (I'm losing my sanity thanks to CodeChef's September Long Contest), and university exams and classes, it's hard to catch some sleep, much less a breather, <strong>so the following article will be brief</strong>. Anyways, back on topic;
</em>
<br><br>
<hr />

Binary exponentiation is a powerful algorithmic technique, to calculate <em>x<sup>y</sup></em> in O(log(y)), as compared to the n<em>aï</em>ve O(y).<sup>
</sup>
<br><br>

Its uses are many. Apart from just being able to easily exponentiate numbers, its use is best served in the implementation of Matrix Exponentiation, which warrants an article of its own.
<br><br>

Now here's the important line -
<p style="text-align:center;">
<strong>All numbers can be expressed as the sum of powers of 2. </strong></p>
This is the concept of the binary number system.
<pre>5 (Decimal) = 4 + 0 + 1 = 1*(2^2) + 0*(2^1) + 1*(2^0) = 101 (Binary)
9 (Decimal) = 8 + 0 + 0 + 1 = 1*(2^3) + ... + 1*(2^0) = 1001 (Binary)</pre>
That being said, the length of this binary equivalent of a decimal number <em>n</em>, will be at most <em>ceil(log(n))+1 </em>where the base of the logarithm is 2. i.e <em>32 </em>when expressed in binary will fit in 5 bits. <em>7</em> in binary, needs 3 bits.
<br><br>
And this is the key, to unlocking the potential of binary exponentiation, as every exponent can be expressed as the product of other smaller exponents that are perfect powers of 2. Let me elaborate --
<br><br>
The core principles that binary exponentiation relies on are
<ul>
  <li>The binary equivalent of a number is at most <em>ceil(log(n))+1 </em>in length</li>
  <li>The high-school math concept of (<em>x<sup>a</sup></em><em>)*(</em><em>x<sup>b</sup></em>) = <em>x<sup>(a+b)</sup></em></li>
</ul>
<h3>The pseudo code, is as follows -</h3>
<pre>scanf("%d %d",&amp;x,&amp;y);
int final_result=1;
int current_result=x;

<span style="color:#0000ff;">//While there are still bits left in <em>y</em> (i.e. while y is not zero)</span>
while(y!=0){

     <span style="color:#0000ff;">//if my rightmost bit(LSB) of <em>y</em> is 1, that implies that</span>
<span style="color:#0000ff;">     //the current exponent contributes to my final result</span>
<span style="color:#0000ff;">     //therefore, I add the current exponent to my final exponent</span>
<span style="color:#0000ff;">     //to do which, I multiply my final_result with my current_result </span>
     if(y&amp;1) final_result*=current_result;

    <span style="color:#0000ff;"> //As I'm moving in bits from right to left, every bit shift </span>
<span style="color:#0000ff;">     //Implies my potential current exponent contribution doubles</span>
<span style="color:#0000ff;">     //Thus, in terms of my current_result, I must square it</span>
     current_result*=current_result;

     <span style="color:#0000ff;">//Shift the bits to the right by 1</span>
     y&gt;&gt;=1;
}

printf("%d\n",final_result);</pre>
<strong>This</strong> is why binary exponentiation takes O(log(n)), as the <em>while</em> loop executes only approx. log(n) number of times, while all the operations inside the loop are O(1), constant in runtime.
<br><br>
The way binary exponentiation works, can be explained better with the example of solving for <em>x<sup>y</sup></em>  given <em>x </em>and <em>y </em>as input.
<br><br>
Consider the example where y=5.
<br><br>
The code flows as follows -
<pre><span style="color:#0000ff;">//Let y=5</span>
while(y){ 
     <span style="color:#0000ff;">//Iteration 1:</span>
<span style="color:#0000ff;">     //The binary equivalent of y is <em>101 </em>right now</span>
     if(y&amp;1) <span style="color:#0000ff;">//This is equivalent to - if(<em>101 &amp; 001</em>) - which is true</span>
          final_result*=current_result; <span style="color:#0000ff;">//this makes final_result=x</span>
     current_result*=current_result;  <span style="color:#0000ff;">//current_result = x<sup>2</sup></span>
     y&gt;&gt;=1; <span style="color:#0000ff;">//y becomes 2 now, as the binary equivalent is now </span><em><span style="color:#0000ff;">10</span>
</em></pre>

<hr />

<pre><span style="color:#0000ff;">    //Iteration 2:</span>
<span style="color:#0000ff;">    //The binary equivalent of y is <em>10</em> right now</span>
    if(y&amp;1) <span style="color:#0000ff;">//This is equivalent to - if(<em>10 &amp; 01) - </em>which is false</span>
         final_result*=current_result; <span style="color:#0000ff;">//This is not executed as no power </span>
<span style="color:#0000ff;">                                       //of 2 contributes to forming x<sup>5</sup> </span>
    current_result*=current_result; <span style="color:#0000ff;">//current result = x<sup>4</sup></span>
    y&gt;&gt;=1; <span style="color:#0000ff;">//y becomes 1 now, as the binary equivalent is now <em>1</em></span></pre>

<hr />

<pre><span style="color:#0000ff;">     //Iteration 3:
     //The binary equivalent of y is <em>1 </em>right now</span>
     if(y&amp;1) <span style="color:#0000ff;">//This is equivalent to - if(<em>1 &amp; 1</em>) - which is true</span>
          final_result*=current_result; <span style="color:#0000ff;">//this makes final_result=x<sup>5</sup></span>
     current_result*=current_result; <span style="color:#0000ff;">//current_result = x<sup>8</sup></span>
     y&gt;&gt;=1; <span style="color:#0000ff;">//y becomes 0 now, as the binary equivalent is now <em>0
</em>     //The loop stops executing</span>
}
printf("%d\n",final_result);</pre>
Thus, in 3 loops, x^5 was calculated. Now, the real beauty of this algorithm is better evident over larger exponents. Such as when it occurs that the million-th exponent takes 20 iterations to calculate, compared to the brute force's 1,000,000.
<br><br>
Now, it's use is best noted in the aforementioned <strong>matrix exponentiation</strong>, which allows one to exponentiate matrices in logarithmic time. This is useful especially when recursive functions can be represented as matrices through some linear algebra, but that will have to be covered in an article of its own.
<br><br>
&nbsp;