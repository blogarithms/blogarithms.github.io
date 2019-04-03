---
layout: post
title: "Why I love and hate Python."
excerpt: "I swing back and forth with my feelings for Python. Here's why."
categories: [personal, development]
image:
    feature: 
comments: true
---

Okay. Right off the bat, I'll state: I enjoy writing code in Python.

That being said -- I also find it VERY annoying at times.

Most of my frustration comes from the frequent issues faced when using Python in programming contests, and so my article will mostly revolve around that.

The moments of joy that Python has given me stem from the development of many projects I've implemented with Python. 

So let's dive in!

## The Pains of using Python

### 1. TLE. Time Limit Exceeded.

![](../../../img/tlepython.gif)

This one is SUCH a pain, I swear. And I'm not talking about when I don't account for an infinite loop, or have a terrible time complexity.

I have received this verdict COUNTLESS times, despite having the expected time complexity, or even otherwise a very reasonable one.

In fact, now when I compete on codeforces, I only submit solutions in Python if it's a Div2A, div2B or div1A question. Any other level? Nope. No Python. It's too slow.

The reason its so slow, is because its an interpreted language. It's high level. It abstracts details from you, hides the nitty-gritty parts of things and allows you to write simple, elegant code in a fluent manner.

And consequently spends a lot of time unravelling what you just effortlessly typed. 

### 2. Immutable Strings.

WHY. WHY ARE STRINGS IMMUTABLE IN PYTHON. 

I get that a strong reason to make strings immutable in Python is so that they can be used as immutable keys in dictionaries -- BUT WHY NOT DO WHAT C++ DOES WITH ITS MAP<> DATATYPE THEN?

C++ performs a deep copy of the keys, when mapping a key to an object -- which sadly goes against Python's general approach to things, which is "copy values -- not memory addresses".

### 3. Non Hashable Lists

Plenty of times, I've seen that I can't use lists as a key in Python dictionaries. 

And yet, C++ provides out-of-the-box compatibility for using vectors as keys in their map datatype. 

*Someone please design a python module to hash lists efficiently. Or point me to a suitable alternative pls, thx.*

![sigh](https://media.giphy.com/media/l0K4jwyp6FZa9phyU/giphy.gif)

------

Okay. Rant over.

Now time for the good bits of Python!

### 1. OPEN SOURCE!

The Python developer community has published SO many wonderful modules, that I feel it could warrant a blog post of it's own. 

*(Maybe I'll write an article on that, too in the future)*

I've seen some super awesome python modules being developed by the community.

From defaultdict, to a python module dedicated to serving XKCD comics, to even <a href="https://github.com/ajalt/fuckitpy">an outrageously named error steamroller</a>, the community for python never disappoints.

### 2. Verbosity

Python is so intuitive to write. At times it feels like I'm writing english.

```python
user = input()
print('Hi, Aditya') if user=='Aditya' else print("You're not Aditya")
```

Now compare this to C++.

```c
#include<bits/stdc++.h>
using namespace std;

int main(){
	string user;
	cin>>user;
	cout<<( (user=="Aditya")? "Hi, Aditya" : "You're not Aditya");	
}
```

There are plenty of memes on the internet about how Python is just pseudocode followed by a ".py" extension.

They have my full support. 

### 3. Versatility

Python is also very versatile, since it's a high level language.

For example, did you know that you can define functions LITERALLY ANYWHERE?

I've seen Python handle function definitions INSIDE iterative loops, or inside other functions.

Think about it -- nested functions! Normal scope rules apply.

Did you also know, that Python can store functions in lists?

Check this -

```python

def foo(a, b):
	return a + b + a*b

def bar(a, b):
	return a**b + b**a

fns = [foo]

print(fns[0](2,3)) # invokes foo(2, 3)

fns.append(bar) # works perfectly
```

I've used Python to build complex machine learning pipelines, automate desktop tasks, send emails, perform graph and social network analysis -- and I've also used Python to spam my friends "Hi" on WhatsApp.

It has catered to every need of mine - complex and useful, to just downright hilarious and unashamed investments of my time.

---

Python certainly has its quirks but I'm glad I find workarounds for them.

So, I hate Python for 3 reasons, while I also love it for 3 other reasons.

And since I seem to be making GIFs a frequent occurrence on my blog now, here's my ending GIF.

![aww](https://media.giphy.com/media/jyn0Uqr9Ov8AmqqK6n/giphy.gif)