---
title: Dealing with really subtle problems and math (day 10)
author: Nick Larsen
categories: streams
date: 2019-02-06 21:05:21
youtube_url: https://www.youtube.com/watch?v=AyVGj0uzAIg
youtube_embed: https://www.youtube.com/embed/AyVGj0uzAIg
---

Today was amazing!  Streaming is so much more fun when there are a ton of people hanging out and making it interesting and really the best part of it is the learning experience for me.  A while back we had changed the goal state for the NPuzzle problem to have the blank in the bottom left hand corner.  Turns out this does _not match_ the literature... opps my bad.  Also this led to a really long and drawn out debugging process.

The hardest debugging sessions are the ones where you expect one thing to be true so much that when it turns out to not be true, you spend the majority of the time questioning your method for determining whether it was true or not instead of accepting it and considering how to find the actual source of the problem.  And you do all this despite the multitude of obvious evidence to the contrary.

In this case, I had previously implemented the NPuzzle problem following the literature ([which you can play with here](https://github.com/NickLarsen/heuristic-search)), and I _knew_ that the states were solvable... because I mean I had done it already.  First we noticed that I was exploring too deep into the search for a particular 4x4 puzzle which I had lifted directly from [the paper](https://cse.sc.edu/~mgv/csce580f12/gradPres/korf_IDAStar_1985.pdf).  Clearly there was a bug.

I started looking for it in so many places, essentially everything I had implemented this time around.  Then one of my viewers chimed in and asked if I had tested if it was solvable or not.  And of course I was like... it's from the paper, it's solvable.  I assumed I'd be able to find my bug if I checked solvability after each swap to see which state it came from, so I implemented the solvability check and low and behold it was saying not solvable right from the start.

Naturally, I must have implemented the check incorrectly.  This is where things got interesting.  Take a peek at the video today and watch me go through round after round of slowly realizing the truth.