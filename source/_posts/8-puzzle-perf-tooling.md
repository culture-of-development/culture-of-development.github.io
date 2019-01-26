---
title: 8 Puzzle Perf Tooling
author: Nick Larsen
categories: streams
date: 2019-01-25 14:35:03
youtube_url: https://youtu.be/LukIOA1JuA0
youtube_embed: https://www.youtube.com/embed//LukIOA1JuA0
---

Today we set out to achieve the 5 goals listed on yesterday's stream:

- ~~redfine the goal state to match what's common in the literature~~
- ~~add some performance tooling, counters for explored nodes and a regular pulsing of performance info~~
- ~~improve the nodes explored per sec by orders of magnitude~~
- implement the manhattan distance metric
- **actually find the optimal solution to the hardest 8 puzzle problem!**

We didn't get quite as far as I had hoped, but we did make good progress and now we can see that using A* with Hamming distance heuristic does infact search deeper than breadth first search.  Overall this is important to convincing us this trek might be worthwhile.

Next week we'll start using a more Github friendly workflow of using issues and pull requests to track progress which will be a learning experience for me and I'll love your input.  On that note, [check out the code the on github](https://github.com/culture-of-development/fast) and feel free to contribute; I'll highlight your work on stream!