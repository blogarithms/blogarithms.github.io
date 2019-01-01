---
layout: post
title: "Switching to a Jekyll blog!"
excerpt: "Why I switched from HTML/CSS/JS to Jekyll, the simple blog-aware static site generator."
categories: [development]
image:
    feature:
comments: true
---

I first started technical blogging back in August 2017. 

At the time, I didn't quite have a strong motive to blog. I just knew that all the ~~cool kids~~ famous developers were doing it.

So, channeling all that excitement, I set up ... a wordpress account. 

![](https://media.giphy.com/media/PJeKg31621Wgw/giphy.gif)

> Yes, I now feel ashamed as a developer who's needed wordpress in the past. Stop sighing in disappointment at me pls.

So anyway, at the time, the focus was on shooting good quality articles out. I didn't really care too much about the domain name, or the UI/UX, or how 'established' it'd make me look.

But eventually, I started noticing that it was the little things as well, that added credibility to my personal brand as a capable developer.

I mean, who'd take a developer seriously if they didn't code up their own technical blog, right?

So, I decided to port my blog onto a GitHub page. 

This was a fairly big move, since in doing so, I had to step away from all of the features Wordpress had. Upon transitioning to my GitHub page, for the next 2-3 weeks, I lost -
- Analytics
- Followers
- Google's SEO/Indexing


And also, I now had the daunting task of designing a nice, pretty UI on my own. Yay me, right?

![frustrated](https://media.giphy.com/media/5Zesu5VPNGJlm/giphy.gif)

Well, ultimately I ended up using Bootstrap templates for rapid prototyping and I ported all my Wordpress articles onto HTML code. (Thank you Wordpress, for making that a one-button task)

But there was one big problem I'd missed while doing all of this.

**I lost Wordpress' Cloud Support.**

Back when I blogged on Wordpress, all my articles were backed up on their cloud. This means, I could use their android app to write articles even on-the-go, during a boring class or late at night when I was too sleepy to boot up my laptop.

But now all that was gone!

![ggwp](https://media.giphy.com/media/xTiTnJ3BooiDs8dL7W/giphy.gif)

So I did the only thing my frustrated, sleep-deprived version of me could do.

Research. (:

I tolerated my current set-up for a bit, but the number of quality articles I was writing was visibly lesser.

At this point, I encountered a new developer's blog.

This blog was clean, and VERY fast to load up. I found out that this was a Jekyll blog.

I soon started reading up on Jekyll, what it was, why people use it, how customizable it is, what could I use it for, etc.

And the tl;dr version is -- its great for developer blogs!

Here's why I'm really enjoying Jekyll -
- WYSIWYG. What you see is what you get. Instant loads when you're writing articles so it wasn't a step back.
- No more HTML. All articles are written in Markdown. Which is sooo elegant, my God.
- Speed. This website scores 90+ on Google's Pagespeed test. My previous HTML/CSS/JS blog scored somewhere between 70-80.
- Design. I find the UI of this blog and many other Jekyll themes just downright beautiful.
- Open-source. Most Jekyll templates are open source, and after tinkering with a few, I'm confident that building one from scratch is also just a weekend's work.
- NO HTML. Only Markdown. Seriously, this deserves to be restated. I find Markdown syntax so intuitive and with a Jekyll blog, I feel like I can fly.

Also, another reason I love Markdown is that it supports HTML syntax, and so because of that, porting my old blog to Jekyll was just a matter of copy-pasting files. Go Markdown!

Aside from this, Jekyll runs on Ruby, and so supports a **bit** of functional logic as well.

If you scroll up to the top of this article too, you'll see below the title, in green text, "X mins read" which is a dynamic variable and not hardcoded into the actual markdown file I'm typing in. This gets generated along with the site when its compiled.

![](../../img/page_read_time.JPG)

My blog assumes that the reader (you!) reads at 180 words per minute on average. :)

Jekyll is also my elegant solution of the timeline you see when you click on <a href="{{ site.url }}/archive/">'all posts'</a> on the sidebar of my blog.

And also, the auto-classification of topics of my articles, when you click on the <a href="{{ site.url }}/categories/">'categories'</a> button. 

Another cool aspect of Jekyll is, that incorporating Google Analytics is just **a single word** that needs to be added into the Jekyll site's configuration YAML file. (That word being your unique Google Analytics ID, of course.)

No more trouble with views, copy-pasting JS code into all subdomains/pages etc. 

Jekyll has helped me spend far less time on the boring, tedious aspects of pushing a blog post online, and helped me focus on what really matters -- the content. And also to a good degree, the user experience. 

Yay! Go Jekyll!

![yay](https://media.giphy.com/media/26vUGPBrOxqIy44CY/giphy.gif)