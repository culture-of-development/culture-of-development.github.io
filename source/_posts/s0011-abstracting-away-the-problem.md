---
title: S0011 - Abstracting away the problem
author: Nick Larsen
categories: streams
date: 2019-02-07 21:24:46
youtube_url: https://www.youtube.com/watch?v=z1QxfXKUYow
youtube_embed: https://www.youtube.com/embed/z1QxfXKUYow
---

Today we started off by updating the goal state to _actually_ match the literature, then we fixed a couple of bugs that popped up once we reran the tests and finally we wrapped up by abstracting out the concept of a problem and making the search solver interface generic.

One key idea for the search algorithms is that they can solve a plethera of problems.  Our implementation up to this point had the actually problem we were trying to solve baked in pretty hard.  I knew if we wanted to make it generic, now was a better time than not because each time we add a new search technique, all we do is increase the time it would take to solve a second problem later on.

This process turned out to be a little complicated and really tested the extent of my C# knowledge.  We had to deal with co/contra-variance, figuring out exactly what the abstraction should look like and some really non descriptive error messages.  Amazingly I wrapped up this code after a marathon 1.5 hour session and IT RAN before the end of the stream!

The result of this was that the NPuzzle problem had been completely eliminated from all files in the search project except for where it was defined.  Now we should be ready to add a second problem... or maybe we'll keep grinding on improving the heuristics for this problem a bit.  In the end we'll need to do both.