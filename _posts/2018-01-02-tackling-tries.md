---
layout: post
title: "Tackling Tries!"
excerpt: "Efficient String Searching In A Dictionary Of Words, How Facebook Searches In Its Database, And More!"
categories: [algorithm]
image:
     feature: 
comments: true
---
<i>Hi! My 4th semester has just started, so I'm trying to type out as many articles ahead of time so that I don't have to keep my large (and partially imaginary) audience waiting. (Shh)</i>

<hr>
<br><br>
Alright, so let's discuss Tries briefly. This article will mainly give you an overview of what tries are, what they can do, why they're used, and why you should consider picking them up.
<br><br>
<b>Why learn tries?</b> Tries are very useful in searching for a given string in a set of strings. Now, that's just the tip of the iceberg but deeper applications of tries may warrant a separate article of its own for another time.
<br><br>
The point here is, tries are efficient, because unlike a naive algorithm to check if a given string exists among <i>n</i> other such strings (which would have a time complexity of O (<i>mn</i>), where <i>m </i>is the length of the search pattern), <b>a trie can determine the presence of a pattern of length </b><b><i>m </i></b><b>in a set of </b><i><b>n</b></i><b> strings in O(</b><i><b>m</b></i><b>). </b>Yeah, proportional to just the length of the pattern.
<h3>Let's get formal!</h3>
<b>Mathematically, </b>a trie is a classic example of a <b>deterministic finite automaton</b>. Fancy words, I agree, let's break this down.
<br><br>
An automaton is a machine that has clearly defined states or 'moments' during execution, with transitions or connections between these states. (Think of it a lot like a flowchart, if you still find this definition vague)
<br><br>
A transition is made from one state to a connected state upon the corresponding transition character occurring next.
<br><br>
An automaton is called "finite" in nature, when there are a limited number of such states. And a finite automaton is deterministic, when there's strictly one possible route between two states for a given transition character.
<br><br>
<img class="aligncenter" src="https://upload.wikimedia.org/wikipedia/commons/thumb/9/9d/DFAexample.svg/250px-DFAexample.svg.png" alt="Image result for deterministic finite automata" width="250" height="150">
<br><br> (An example of a DFA, where transitions occur only when zeroes are fed into the states.)
<br><br>
So now that you have a broad idea on what a DFA is, I'll make my next leap in statements -- <b>A trie is a deterministic finite automaton, where each state refers to some substring of some string present in the set of strings</b>.
<br><br>
Additionally, a DFA has an (or more than one) 'end' state and in the case of a trie, the last character of a string is the end state of that string.
<br><br>
Basically, what that means, is this -

<img src="https://qph.fs.quoracdn.net/main-qimg-aea35028c2d1fe08e27cb3bda001c41f-c" alt="Image result for trie">

This is a pretty good representation of how I picture a trie. Basically, in the given image, picture each node as a state, and to transition from one state to the next, the transition variable is the character on the next node.
<br><br>
This means, to transition from the <i>START</i> state to the "<i>A</i>" state, I must input an 'A' character and then my machine will undergo the corresponding transition.
<br><br>
Thus, you can see that as I parse the rest of my pattern string, I will have four cases -
<ul>
	<li>I reach a leaf node <b>after</b> completely parsing my pattern -- implying there's an exact match and the pattern string exists in the source set of strings.</li>
	<li>I reach a leaf node <b>before </b>completely parsing my pattern -- implying my pattern string is a longer string than what's already present in my source set of strings. (For example, what happens, if I try searching for "DOTTED" in the above picture)</li>
	<li>I do not reach a leaf node after completely parsing my pattern -- implying I'm searching for a pattern which doesn't occur separately and at best is a substring of a string that is in my source set. (Try searching for "NO" in the above trie)</li>
	<li>I find no suitable transition as no such connection exists. (For example, try "EAT")</li>
</ul>
Now, of course, you can implement the dynamic, pointer-based tree structure too but I particularly like the following two-dimensional array based static implementation of the data structure because of its short size (<i>~20 lines</i>, <i>both inserting and querying</i>)
<br><br>
So let's get started!
<br><br>
<hr>

<h2>The Implementation</h2>
Okay, so tries have two functions -
<br><br>
<b>insert( ) </b>- The insert function is invoked when you're creating the trie and inserting the strings of your source set into the trie, and creating the states and transitions.
<br><br>
<b>find( ) </b>- The find function is invoked when you've created your trie and are now checking if a given string is present in your source set or not by transitioning through your trie and effectively thus, checking your source set.
<br><br>
<h3>insert( )</h3>

```c++
//Firstly, the relevant variables -
//Store the transitions for a given state and character
int trie[26][1000];

//A boolean array to tell us if a state has been initialized or not
bool created[1000];

//An array to tell us if a given state is an end state or not
int end[1000];

//To track the newest state
int sz=0;

//Function to insert a string <i>s </i>into the trie
int insert(string s){
     //Initial state of trie
     int v=0; 
     
     //Iterating over the parameter string
     for(int i=0;i < s.length();i++){
          
          //Identifying the transition variable
          int c=s[i]-'a'; 

          //If the state is not yet created
          if(!created[trie[c][v]]){

               //Create it and label it the newest state
               trie[c][v]=++sz; 
         
               //Update the corresponding <i>created </i>hash table
               created[sz]=true; 
          } 
 
          //Transition to the next appropriate state
          v=trie[c][v]; 
     } 

     //Mark the end state as valid
     end[v]++;
}
```

And that's how insert works. The reason we took the <i>trie[ ][ ] </i>array to be a 26x1000 array is because there are 26 characters we are considering and we are assuming the maximum number of nodes is 1000, (a reasonable assumption as most problem statements specify the maximum number of characters which in the worst case equals the maximum number of nodes).
<br><br>
<hr>
<br><br>
<h3>find( )</h3>
The find( ) function traverses the trie with a given string and checks if the given string is in the trie or not. Here's how it goes -

```c++
int find(string s){

     //Define initial state of trie
     int v=0;

     //Iterate over given string
     for(int i=0;i < s.length();i++){

          if(!created[s[i]-'a'])
            return false;
          v=trie[s[i]-'a'][v];
     }
     return end[v]>0;
}
```

<br><br>
And that, is how you implement the find( ) method to check whether a given string is present in a set of strings.
<br><br>
<hr />
<br><br>
<i>The first time I came across tries was in a very straightforward problem on a CodeChef Long Challenge - </i><a href="https://www.codechef.com/MAY17/problems/WSITES01"><i>Blocked Websites (WSITES01)</i></a><i>, and after the contest upon reading the editorial, did I discover this cool data structure.</i>
<br><br>
<i>Anywho, I just churned out 1k words on this, and now imma hit the bed. Ciao.</i>
<br><br>
<i>Aditya</i>