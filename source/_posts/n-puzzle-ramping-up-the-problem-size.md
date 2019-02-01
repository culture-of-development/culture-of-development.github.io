---
title: 'N-Puzzle: Ramping up the problem size (day 9)'
author: Nick Larsen
categories: streams
date: 2019-02-01 17:15:58
youtube_url: https://www.youtube.com/watch?v=CJrC-KVXnx0
youtube_embed: https://www.youtube.com/embed/CJrC-KVXnx0
---

Today we started off with the goal of trying to ramp up the problem size for the N-Puzzle problem.  Under our previous implementation, we would have had to go from a state space size of 9! to a statespace size of 16!, or roughly an 8 orders of magnitude increase.  That's a little too baller for where things are right now, so instead I wanted a way to ramp up the problem size a bit less.  To do this, we relaxed the constraint that the board must be square an instead let there be differing height and width.

This conversion turned out to be a good bit easier than expected and after we were done we ran the full test suite... which highlighted a bug in `OpenSet`.  Yea, another bug in that thing.  This one turned out to be easier to fix because I had just left out one condition where were not correctly removing states from the set.

After this we were able to solve the 3x4 puzzle problem with manhattan distance very quickly.  We ramped it right up to one of the easier 4x4 grids and then it did not complete... and eventually we killed it.  We spent some time tooling it to see the output heartbeat and what we noticed was that the speed was quickly dropping off after the first 100,000 or so states made it in the system.  This just became our TODO for next week.