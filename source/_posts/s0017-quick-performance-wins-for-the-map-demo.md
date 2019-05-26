---
title: S0017 - Quick performance wins for the map demo
author: Nick Larsen
categories: streams
youtube_url: 'https://www.youtube.com/watch?v=9s_VoAG7OoA'
youtube_embed: 'https://www.youtube.com/embed/9s_VoAG7OoA'
date: 2019-02-22 22:27:17
---

Yesterday life came up and I had to bail unexpectedly on the stream, so today I went for a double length and tried to get 2 things done instead of one.  I'd say we made it through roughly 1.5 things :).  In case you're curious what came up, my son decided to administer my daughter's first ever haircut using some kitchen shears he found on the counter.  My wife was overcome with a large number of emotions; I had to run interference since it's my job as a parent first and foremost to keep everyone alive.

Today's tasks were to improve the time it takes to find the nearest points to the locations clicked on the map for the start and end of the directions and to improve the time it takes for the search to complete.  The first task I knew would be fairly easy because it's a well documentated problem, but the second problem was going to be more difficult and it turns out I was correct on both counts.

Improving the speed of finding the closest location is called a [nearest neighbor search](https://en.wikipedia.org/wiki/Nearest_neighbor_search) and there are lots of ways to accomplish this task.  One of the simplest approaches, and probably the most prominent technique, is to use a [K-D tree](https://en.wikipedia.org/wiki/K-d_tree).  In the average case a K-D tree can find the single nearest neighbor in `O(log n)` compared to the `O(n)` time full scan we were doing.  In the New York dataset there are roughly 708,000 nodes, and the log base 2 of that is roughly 20.  You can see how even with a little bit of overhead this is a huge peformance improvement and it showed in the numbers where the scan was taking roughly 330 milliseconds, the K-D tree takes only 0.2 milliseconds on average.  That was a huge win.

Improving the speed of the search is a much more difficult problem and there are lots of ways to do it.  The approach I decided to take was to improve the heuristic that the search used to estimate the remaining path length.  The simple approach we used previously was the straight-line havesine distance.  This works well for small area graphs like the one's we've been using, however when you expand the graph to say the entire United States, all of a sudden you're searching way too many nodes for each search.  I decided to use the ALT algorithm, as outlined in [Computing the shortest path: A search meets graph theory (Goldberg and Harrelson 2005)](http://www.academia.edu/download/5622730/10.1.1.136.1062.pdf).

ALT stands for A* search, landmarks heuristic, triangle inequality which are the main components of the algorithm.  Additionally one other major component from the paper is bidirectional A* search, which we have not yet attempted.  In this stream I worked on the landmarks portion of this paper and holy cow did we run into some problems.  Have a look and watch the highs and the lows as we learn a ton in this 4 hour stretch.