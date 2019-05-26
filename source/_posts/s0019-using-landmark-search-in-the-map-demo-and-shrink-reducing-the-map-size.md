---
title: S0019 - Using landmark search in the map demo and shrink reducing the map size
author: Nick Larsen
categories: streams
youtube_url: 'https://www.youtube.com/watch?v=V5jStJ6FBXY'
youtube_embed: 'https://www.youtube.com/embed/V5jStJ6FBXY'
date: 2019-02-27 23:13:28
---


Over the weekend I put considerable effort into trying to prove that landmark heuristics were inadmissible and I came up with an example that clearly showed there is a pathalogical graph which the method we had implemented for the landmark heuristic is inadmissible because it overestimates the actual distance to the goal.  I was fairly certain that landmarks were supposed to be addmissible so I revisited the original paper to give it a more critical eye and sure enough the method we used was only correct for undirected graphs, but it fails misterably for directed graphs.  It took a few tries to get it all right but I finally got all the tests to go green again.

On stream the plan was to get this all hooked into the demo map and see if we accomplished our goal from last time of getting the time it takes to run a search down significantly.  What we were able to verify is that this was reducing the number of nodes explored however it was not doing so dramatically enough to reduce the time a noticable amount.  So basically the grand plan was not quite achieved, but that's not the end of the road of course.  There's still bidirectional A* and there are other options as well.

The first thing I tried to tackle today was to reduce the graph to only contain intersections.  Along many of the roadways, especially the highways, there are long stretches of points that are necessary to build correctly show the curve of the road, however the points only have one way in and one way out.  Think about an interstate.  There are long sections of windy road where you realistically cannot change direction.  The only real place to change direction is when you reach an exit, which only happens every so often.  In more rural aread it might be as much as 10 or 20 miles between exits, but there are still going to be tons of twists and turns that need lots of points to correct show on the map.  The idea is to reduce the map so that all of the non intersection points are removed and it's just a map that consists only of intersections.  I ran out of time while working on this and had to set it aside.
