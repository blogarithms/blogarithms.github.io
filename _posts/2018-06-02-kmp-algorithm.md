---
layout: post
title: "Knuth-Morris-Pratt (KMP) Algorithm"
excerpt: "Pattern Matching in linear time! This algorithm is used in network security, genome encryption and in many other real world scenarios!"
categories: [algorithm]
comments: true
---

The KMP Algorithm is a powerful <b>pattern matching algorithm </b>that executes in linear complexity.
<br><br>
Here's what that means -
<br><br>
Let's say I have a source text I'm trying to find a certain substring in. Let this source text/string have a length <i>n </i>and let my substring I'm searching for in the source text, have a length <i>m. </i>
<br><br>
Now, the naive simple nested-for-loop code will give you a complexity of O (m*(n-m+1))
<br><br>
<b>The KMP Algorithm does this in time O (m+n)</b>
<br><br>
Oh, one drawback though, it also <b>has</b> <b>a space complexity of O (m) </b>because there's some pre-processing involved.
<br><br>
So now that you know how powerful it is, let's dive in! I'll be splitting this article into 2 sections -
<ul>
    <li>Pre-Processing</li>
    <li>String Matching</li>
</ul><br>
So let's start off with the pre-processing -- <b>the longest prefix suffix array</b>.
<br><br>
<hr />

<h2>The Pre-Processing</h2>
This is the part of the algorithm which fetches a space complexity of O (m) and a time complexity of also O (m)
<br><br>
The pre-processing phase is where we identify certain properties of the given pattern and store certain information we'll find handy later on.
<br><br>
This "information" we store is the <b>length of the longest prefix which is also the suffix. </b>
<br><br>
I'll elaborate.
<br><br>
Let <i>lps[ i ] </i>represent the length of the longest suffix that is also a prefix, in the first <i>(i) </i>characters. Here's an example.
<br><br>
Let my pattern string be P =<i>"abcdabcde"</i>
<br><br>
Now here are all the prefixes, with the longest prefix suffix, highlighted in bold -
<br><br>
a
<br>
ab
<br>
abc
<br>
abcd
<br>
<b>a</b>bcd<b>a</b>
<br>
<b>ab</b>cd<b>ab</b>
<br>
<b>abc</b>d<b>abc</b>
<br>
<b>abcdabcd</b>
<br>
abcdabcde
<br><br>
And they're corresponding <i>lps [ ] </i>values are = { 0, 0, 0, 0, 1, 2, 3, 4, 0 } since we want the length of the bold suffix prefix.
<br><br>
For now, let's just operate under the assumption we need this longest prefix suffix array. You'll see why we need it in the second phase.
<br><br>
But for now our task is to generate this <i>lps</i> array optimally.
<br><br>
Obviously, an O(|m|^2) solution is evident where we check the length of matching suffixes for every prefix but we can't have that. We need to perform this in O(|m|) and that's the key part of this pre-processing.
<br><br>
For the duration of this article I'll coin the term "prefix-suffix" which would refer to the existence of a substring that is both a prefix AND suffix of the string.
<br><br>
The way we accomplish this is by doing the following -
<ul>
    <li>We maintain two integers <i>i </i>(initialized to 1) and <i>j </i>(initialized to 0)</li>
    <li>Now j keeps repeatedly being pointed back to the potential start of a suffix which is a prefix-suffix with relevance to the current character (that <i>i</i> points to) - Remember, i is always ahead of j. Now visualize and re-read this point. (i.e. in abcdabcd, when i equals 4, j will point to 1, as index 0 is the same character as that of index 4 and the next alternate potential matching character will be 0+1 = 1)</li>
</ul>
<ul>
    <li>Now, we iterate over the given pattern to construct the <i>lps</i> array and we do this as follows -</li>
    <li>If <i>pat[ i ] == pat[ j ]</i>, we assign <i>lps[ i ] </i>to <i>(j+1) </i>since we know that our current character <i>pat[ i ] </i>could also serve as <i>pat[ j ]</i> and so when we're doing the actual string matching in the next phase, the <i>(i+1)</i>th character could refer to even the <i>(j+1)</i>th character as ambiguity arises in where to locate the string when the first character matches. You'll see in the next point.</li>
    <li><br>If <i>pat[ i ] != pat[ j ] </i>then we have two cases.</li>
    <li>If <i>j != 0</i> then we update <i>j </i>as <i>j = lps[ j-1 ] </i>because we may be referring to a different occurrence of the previous character in the given pattern <br>and there's still a chance a common prefix suffix can exist and we were looking at the wrong place. (The case of ambiguity mentioned above)</li>
    <li>If <i>j == 0,</i> well then this symbol is occurring for the first time or shares no common prefix-suffix, for example the last characters in "abcd" or "abcae" have the length o<br>f their longest prefix suffix as zero. Proceed to the next character, meaning <i>i++</i></li>
</ul><br><br>
And that's it! We're done with the pre-processing phase this way!
<br><br>
We now have our <em>lps</em> array ready in O(length of pattern). Here's how that goes in implementation -<br><br>
```c++
vector<int> build(string pattern){
     int m=pattern.length(); //Find pattern length
     vector<int> lps(m,0);  //Declare lps array 
     int i=1;  //init forward pointer
     int j=0;  //init backward pointer
     while(i < m){
          if(pattern[i]==pattern[j]){  //if forward equals backward
               lps[i]=j+1;  //the current character at forward can also lead to the next backward character 
               i++;  //go to next forward
               j++;  //go to next backward
          }
          else{ //if forward does not equal backward
               if(j!=0)  //if length of prefix isn't zero, we go to prev backward prefix of same character 
                    j=lps[j-1]; 
               else  //Else if j equals zero, i.e. if there is no prefix
                    lps[i++]=0;  //assign corresponding length and go to next forward
          }
     return lps;
}
```

<b>Phase 1 complete. </b>
<br>
<hr />
<br>
<h2>The string matching</h2>
Okay, so now we've created our <i>lps</i> array in linear time. Now let's leverage that to find if our pattern appears in our given string.
<br>
We do this by following the algorithm below -
<ul>
    <li>Declare two integers <i>i</i> and <i>j </i>and initialize both of them to zero. <i>i</i> here points to the text and <i>j</i> here points to the pattern's index.</li>
    <li>Now as we iterate over the text, we have two cases -</li>
    <li>Case 1: patter<i>n[ j ] == text[ i ] </i>where in we just go to the next characters in both the text and the pattern. <i>i++; j++;</i></li>
    <li>Now if <i>j == pattern.length()</i> then we have traversed the entire pattern meaning we have just come across an entire occurrence of the pattern. Yay! Also, now we reinitialize <i>j</i> to <i>lps[ j-1 ] </i>in the event that the last character of the pattern is part of a prefix of the same pattern (i.e. the next occurrence of the pattern has already begun.)</li>
    <li>Case 2: the current pattern character does not match the text character, then if <i>j </i>is not zero, we update it to <i>lps[ j-1 ] </i>and if j is zero, just proceed to the next text character.</li>
</ul><br>
And that's it! Ultimately, you'll exit your loop and at which point you'll have come across every potential occurrence of the pattern in the given string.
<br>
That's how you match patterns in linear time! :)
<br>
And to wrap up, here's my C++ implementation.<br>

```c++
int check(string pat, string txt, int n, int m){
     int i, j; 
     i = 0; j = 0; 
     vector<int> lps = build(pat); 
     while(i < n) { 
          if(pat[j] == txt[i]){ 
               i++; j++; 
          } 
          if(j==m){ 
               cout<<"Found!\n"; 
               j = lps[j-1]; 
          } 
          if(j < m & i < n && pat[j]!=txt[i]){ 
               if(j!=0)j = lps[j-1]; 
               else i++; 
          } 
     } 
     return 0; 
}
```

<hr />
<br>
<i>That's that, it takes a while to understand the flow of the code but imitating it on pen and paper definitely helps. </i>
<br><br>
<em>Cheers,<br>
Aditya Ramesh</em>  
<br><br>