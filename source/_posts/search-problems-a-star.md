---
title: 'Search Problems: A*'
author: Nick Larsen
categories: streams
date: 2019-01-24 14:07:35
youtube_url: https://www.youtube.com/watch?v=c2rR3jbxj0c
youtube_embed: https://www.youtube.com/embed/c2rR3jbxj0c
---

Today we continued out trek on search problems.  We implemented [breadth first search](https://en.wikipedia.org/wiki/Breadth-first_search) and [A*](https://en.wikipedia.org/wiki/A*_search_algorithm) using [Hamming distance](https://en.wikipedia.org/wiki/Hamming_distance) as the heuristic which we applied to the 8 puzzle problem.  We verified the correctness of finding the optimal solution by solving problems with only a few moves to reach the goal.  Once that was done, we ramped up to the hardest problem for the 8 problem and saw that even my beefy ass computer with 128 GB of ram was still easily resource exhausted using both of these techniques.  

For reference, the harest 8 puzzle problem is (0 is the blank) ([source](http://w01fe.com/blog/2009/01/the-hardest-eight-puzzle-instances-take-31-moves-to-solve/)):

> 8 6 7  
> 2 5 4  
> 3 0 1  

Try to solve that in 31 moves by hand!

The last 5 minutes really sums up how incredible of a challenge it is going to be to find the optimal way to solve matrix multiplication using search techniques.  This game only has 9! solvable states (362,880 / 2) and somehow we managed to use a technique which explores 10<sup>12</sup> states.  That's 7 order of magnitude too many!  Finding good heuristics is going to be key if we're going to have any chance of completing our goal of finding a new state of the art way to do dense matrix multiplication.

The todo list for tomorrow is:

- redfine the goal state to match what's common in the literature
- add some performance tooling, counters for explored nodes and a regular pulsing of performance info
- improve the nodes explored per sec by orders of magnitude
- implement the manhattan distance metric
- **actually find the optimal solution to the hardest 8 puzzle problem!**