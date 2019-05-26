---
title: S0008 - Implementing an OpenSet for AI search algorithms
author: Nick Larsen
categories: streams
date: 2019-01-31 17:07:58
youtube_url: https://www.youtube.com/watch?v=7ZDgtKfW4wA
youtube_embed: https://www.youtube.com/embed/7ZDgtKfW4wA
---

Today we fixed our `OpenSet` implementation and chatted a bit about some myths about AI in general.  We had a fair amount of dicussion today and it was blast!  Sorting out the details of the open set required using two collections, one to quickly load up the min item, and another to maintain a fast lookup of state costs.  This class ended up being a fairly typical programming exercise and you can watch me go through various phases of frustration and raw joy as we finally got it working.

With this done, we spent a fair amount of time grinding through some example problems to see how the heuristics affect the number of nodes you have to explore in order to find the goal.  Tomorrow we'll see if we can ramp up the difficulty by increasing the size of the board.