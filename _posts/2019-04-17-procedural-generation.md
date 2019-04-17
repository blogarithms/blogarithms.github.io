---
layout: post
title: "Building My Own Procedural Generation Algorithm"
excerpt: "How Minecraft, No Man's Sky and other video games simulate unreasonably large planets!"
categories: [algorithm, development]
image:
    feature: 
comments: true
---

I've been a big fan of video games for quite a while. They were an integral part of my childhood.

And, while I did occasionally enjoy the major multimillion dollar budget video games like Grand Theft Auto, or Call Of Duty, I loved playing indie games because in my experience they overflow with developer love.

Sadly though, in the last few years, owing to a TON of programming, writing and college work, I haven't gotten to play too many indie games and I've got some catching up to do.

On that list of indie-games-I-have-to-catch-up-on, was a <a href="https://en.wikipedia.org/wiki/Roguelike" target="_blank">roguelike</a> cute indie game called Spelunky.

![Spelunky](https://i.imgur.com/WDY9AG2.jpg)

Spelunky is an Indiana-Jones style action-adventure 2D platformer, where you're an archaelogist who's descending the depths of mysterious caves in search for treasure and secrets.

However the game had a "permadeath" mechanic, meaning that if your character dies in the game, they lose all progress and you have to start afresh -- very real-life like.

This made me put thought into every decision I made -- from jumping over a small pit for that extra treasure chest, to diving into seemingly clear waters.

Now, some of you might be wondering "Okay, but even if you die - you already know what those first few levels are going to be like, so over time you'll be able to clear all levels just by muscle memory, and not skill. Right?"

The answer to this question brings me to the point of this blog post.

"No."

----

This is because, Spelunky, like many other roguelike video games, employed a technique called "Procedural Generation".

It's the idea of "building-as-you-are-going". 

Consider Minecraft. Minecraft is HUGE. There are YouTube videos attempting to map its scale, calculate the real-life analogies etc, but from a developer's viewpoint, there's absolutely **no way!** for a computer to load up all that onto the RAM.

So how did the developers allow players to explore such large worlds while simultaneously not nerfing all the RAM it had access to?

Procedural Generation.

Even the fairly recent "No Man's Sky" that features 18 quintillion life-size planets, relies on procedural generation algorithms in order to determine planet size, gravity, flora-fauna characteristics, etc.

![nomanssky](https://i.imgur.com/v0f3wY5.jpg)

Onto the topic of this blog post.

I've always wondered how such procedural generation algorithms would work, and so for the past few days, I've been spending time perusing the existing work in the field.

As it turns out, I discovered that procedural generation is fairly linked with genetic and evolutionary algorithms, and this technique is even used in modelling cities or ecologies.

So, while thinking over how I'd build my own procedural generation algorithm - I ran into a roadblock.

"2D or 3D?"

I decided I'll start off with 2D, and see what that's like first -- and that's also just about what this blog post will explore.

-----

#### Generating 2D Worlds

The first kind of world I decided to generate was the kinds that all the Gameboy pokemon games were like.

The ones where the camera was overhead, and there was no 3D aspect to the world.

![pokemon](https://i.imgur.com/zCHxX9Y.jpg)

For convenience I decided to make a dungeon crawler map, first.

I approached this by generating a 2D integer grid, where each coordinate would correspond to a position on the map - and the value on the index would determine what was on the actual map.

My notation implied, if the value was a 1, then the map would have a roadblock at this point. Else it was an open space, where the character could walk through.

But how do I automate this?

Easily. :)

Firstly, I'll need to provide a seed - and this is my key. 

My dungeon will be random, yeah - but it'll be mapped to my seed. Every time I reuse this seed, the same dungeon will be generated. This way, I can be sure that when the player revisits the same location twice, the area doesn't change and its still the same as it was before the player left.

Ideally, if I were building a full-fledged game, I'd map my seed to the geographic coordinate of my location in the game so that every time I revisit the dungon, the same seed is fed into the random algorithm.

```python

import random

# assuming we're in the 40th floor of a dungeon
random.seed(40)

# an n x n dungeon
n = 20

```


Now for the actual algorithm -

Let's assume we're dealing with an NxN dungeon.

```python

dungeon = [[0 for i in range(n)] for j in range(n)]

```

Each cell of the dungeon is either a 1 or a 0. 

A 1 implies a rock or an obstacle, while a 0 implies open, traversable area.

We'll define the starting position to be (0,0) and the end to be (n-1, n-1)

```python

start = (0, 0)
end = (n-1, n-1)

```


Let's populate our dungeon with obstacles, with a probability of 40% that they appear on a random cell.

```python

for i in range(n):
    for j in range(n):
        obstacle_prob = random.randint(1, 100)/100

        # placing an obstacle with a probability of 40%
        if obstacle_prob <= 0.4:
            dungeon[i][j] = 1

dungeon[0][0] = 0
dungeon[n-1][n-1] = 0

```

I wish I could say we're done, but we're not -- because we need to verify if the dungeon we just created is actually traversable or not. We wouldn't want to give our player an unbeatable dungeon, now would we? :)

How do we do that? How do we check if there exists a path from the start to the end?

Well, I can do that with either a breadth-first-search or a depth-first-search or with a disjoint set union find algorithm, after all, this is just a simple maze path finding problem now.

So let's use DFS to solve our problem.

Modelling this maze as an implicit graph with each cell as a node linking to all 8 of its neighbors, we get this -

```python

# determining if the generated dungeon is valid or not with a DFS
visited = [[0 for i in range(n)] for j in range(n)]

def issafe(curx, cury):
    return curx>=0 and curx<n and cury>=0 and cury<n

reached = 0

def dfs(curx, cury, parx, pary):
    if curx==end[0] and cury==end[1]:
        global reached
        reached = 1
        return 1
    else:
        visited[curx][cury] = 1
        ans = 0
        for x in [curx-1, curx,curx+1]:
            for y in [cury-1, cury, cury+1]:
                if x==curx and y==cury:
                    continue
                if issafe(x, y) and dungeon[x][y] == 0 and visited[x][y]==0:
                    ans |= dfs(x, y, curx, cury)
                    visited[x][y] = 1
        return ans
    
dfs(0, 0, -1, -1)

```

I've added a global variable "reached" which will tell me if I've reached my end state during the DFS and put it right in the base condition.

But again, this doesn't solve our problem! This just tells us if our dungeon is valid or not.

To actually **fix** a broken dungeon, I borrowed an idea from image processing -- *dilation*.

I'll simply dilate all empty cells. Dilation is the process of basically "bloating up" certain pixels of an image and overwriting its immediate neighboring pixels with itself.

Here, I'll overwrite the surrounding cells with the same "open" state, regardless if they have an obstacle or not.

I'll keep dilating until there's a path to the exit. :)

```python

while reached==0:
    dfs(0, 0, -1, -1)
    visited = [[0 for i in range(n)] for j in range(n)]
    if reached == 1:
    	break
    
    # if dungeon is untraversable, we perform dilation
    for i in range(n):
        for j in range(n):
            if dungeon[i][j] == 0:
                for x in [i-1, i, i+1]:
                    for y in [j-1, j, j+1]:
                        if issafe(x, y):
                            dungeon[x][y] = 0

``` 

And voila! We're done! Just like that, we procedurally generated a valid maze in our own way, for a given seed.

```

C:\Users\Aditya\Desktop\blog\blogarithms drafts\Procedural Generation> python .\procgen.py
0 1 0 0 0 0 1 0 1 0 0 1 0 0 0 0 1 0 1 0
0 1 0 0 1 0 1 1 0 1 1 0 0 0 0 0 1 1 1 0
0 1 1 0 0 1 0 0 1 0 0 0 0 1 0 1 0 1 0 1
0 0 0 0 0 0 0 0 0 0 1 0 0 1 1 0 1 0 0 0
0 0 1 1 1 1 0 0 0 0 0 0 0 1 1 0 0 1 0 1
1 0 0 1 0 1 0 0 1 0 0 1 1 1 0 1 1 0 1 0
0 0 0 0 1 0 0 0 1 0 1 0 0 0 0 1 1 0 0 0
1 1 0 0 1 0 1 1 0 0 1 1 1 0 1 1 0 1 0 0
0 1 1 0 0 1 0 1 1 0 0 0 0 0 0 1 0 0 0 1
0 1 0 1 1 0 0 0 0 0 0 1 0 1 0 1 1 0 1 0
1 1 0 1 0 1 1 1 0 0 1 0 0 0 0 1 0 1 0 0
0 1 0 1 0 0 0 0 0 0 0 0 1 1 0 0 1 1 1 1
0 0 1 0 1 0 0 1 1 1 0 0 0 0 0 0 1 1 0 0
0 0 0 0 1 1 0 0 0 1 0 0 1 0 1 0 1 1 0 1
0 0 0 0 0 0 1 1 1 0 0 0 0 1 1 0 0 0 0 1
0 1 1 1 0 0 1 1 0 0 1 1 0 1 0 0 0 1 0 0
0 0 0 0 1 0 0 1 0 1 0 0 0 0 0 0 0 0 0 1
1 0 0 0 1 0 0 0 1 0 0 0 0 0 1 1 1 0 1 0
1 1 0 1 0 0 1 0 0 0 1 0 0 0 1 1 1 0 1 0
0 0 0 0 0 0 1 0 0 0 0 0 0 0 1 1 1 1 1 0

```

To make your own dungeon crawler game in the same manner, all you'd need to do next is map or hash the seed to the level number, and spawn a few enemies at random cells. :)

Of course, this is barely the surface. With terrifyingly theoretical books, lectures and research work going on in the subject, this article in no way attempts to compete with those. 

This is simply how I'd build my own procedural generation algorithm in the middle of the night. :)

(I'm not kidding - check out the timestamp of the screenshot below 😛)

![Timestamp](https://i.imgur.com/3gMWnIL.png)



Cheers! And you can find all code from this blog post over <span style="color:blue">[here - https://github.com/RameshAditya/Procedural-Generation](https://github.com/RameshAditya/Procedural-Generation)</span>

----------------------------------------------------------------------------------------

For more of my work, follow me on:

LinkedIn: <span style="color:blue">[https://www.linkedin.com/in/adityaramesh1998/](https://www.linkedin.com/in/adityaramesh1998/)</span>

GitHub: <span style="color:blue">[https://github.com/RameshAditya/](https://github.com/RameshAditya/)</span>

Twitter: <span style="color:blue">[https://twitter.com/adityaramesh98](https://twitter.com/adityaramesh98)</span>