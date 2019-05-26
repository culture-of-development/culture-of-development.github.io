---
title: 'S0020 - Graph reduction: intersections only road map'
author: Nick Larsen
categories: streams
youtube_url: 'https://www.youtube.com/watch?v=GI0jr82H5qg'
youtube_embed: 'https://www.youtube.com/embed/GI0jr82H5qg'
date: 2019-03-05 09:08:08
---


After taking yesterday off to _finally_ fix a bug I've been looking into for three days (narrator: it was a typo), I'm back at it today for a double length stream!  The goal is to reduce the number of map nodes by eliminating all the points between intersections.

In real life, many roads are windy and curved.  To make sure that these roads can plot correctly when drawn, the map data contains numerous points for each road segment so that the tool that draws the map can draw them correctly.  From the standpoint of the directions finding problem, the only thing that matters, however, is decision points, i.e. which direction should I go next from this intersection.

All those little bitty road segments are not decision points, in fact, most of the time you have to keep driving the same direction though many of them.  This is especially easy to see in the case of interstate highways where the exits are miles apart and the roads can have a lot of curves between the exits.  The concept here is that be eliminating those false decision points, we can improve the speed of our search.

Follow along as we work though this a few times, run into lots of difficult to understand bugs and ultimately reduce the size of the graph by roughly 40%.  After the stream ended I realized how much smaller we can make this as well, so hopefully we'll have time to come back and work on that soon.
