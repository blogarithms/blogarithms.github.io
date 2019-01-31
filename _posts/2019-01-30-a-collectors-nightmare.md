---
layout: post
title: "Some math behind collectibles!"
excerpt: "Taking a look at the Coupon Collector's Problem"
categories: [algorithm]
image:
    feature: 
comments: true
---

This will likely be a short blog post, as an attempt to reinforce a very quick concept I picked up recently.

Its surprising how I never gave this thought before, but now that I have, I thought I'd document it for either my future reference or for any casual, interested readers.

This article will be slightly more math-oriented and less algorithmic. That being said, let's jump in!

----

As a child, I often used to buy happy meals from McDonalds' or buy chips and biscuit packets because of the prospect of either finding a new collectible toy, or a new action figure, or whatever the trend was at the time.

Recently I decided to get math-y about it and determine just what the **expected number of purchases** I'll have to make, to obtain ALL collectibles of a certain trend.

I wanted to quantify this as a function of N, if there were N collectibles.

And as it turns out, this is a pretty nice and well established problem in math, dubbed the "coupon collector's problem".

The methods employed in calculating the expected results are pretty clever.

Let's define a random variable Xi that defines the probability of getting a brand new collectible after already possessing (i-1) collectibles.

This obviously implies that X1 = 1 (since the first collectible you get is always unseen)

This also implies X2 = (N-1)/N

And when you generalize it, you can express Xi as:

```

P(selected coupon is the ith new coupon) = (N - (i - 1)) / N

```

Now, this is the probability of collecting the ith new collectible. In other words, this is a success.

While the failure, has a probability of (i-1)/N. Or basically (1-p) where p = P(new coupon=Xi).

This is important, because this helps us see that in order to collect Y new collectibles in total, we need to have a total of Y successes.

Using the property of [Linearity of Expectation](https://en.wikipedia.org/wiki/Expected_value#Linearity), since we've defined Xi as the probability of getting "the ith new collectible", we can say that:

```python

E(unique collectibles == N) = 1*E(X1) + 1*E(X2) + ... 1*E(Xi) + ..... 1*E(XN)

```

This is because, expectations can be added up. That's what linearity of expectation means, and that's its whole premise.

Now, we know that P(collecting i unique collectibles) is an exponential function involving (i-1) losses and 1 success.

We've defined this success as P(new coupon = Xi) and loss as (1-P(new coupon=Xi)).

And in general, we know that the expected value of a geometric distribution is 1/success.

This means, that E(Xi) = 1/(P(new coupon = Xi))

Implying E(Xi) = N / (N - (i - 1))

Thus, in general, the number of purchases one would have to make in order to obtain all N collectibles, is -

```python

purchases = N * (1/1 + 1/2 + 1/3 + 1/4 + ... + 1/(N-1) + 1/N)

```

So the next time you're getting a happy meal, be prepared to buy the remaining to likely see all your collectibles. 😉

---------

Follow me on GitHub, and LinkedIn to stay tuned to more technical/algorithmic blog posts! :) 

GitHub: [https://github.com/RameshAditya](https://github.com/RameshAditya)

LinkedIn: [https://linkedin.com/in/adityaramesh1998](https://linkedin.com/in/adityaramesh1998)

Blog: [https://blogarithms.github.io/](https://blogarithms.github.io/)

Portfolio: [https://rameshaditya.github.io/](https://rameshaditya.github.io/)