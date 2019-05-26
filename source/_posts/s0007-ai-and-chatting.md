---
title: S0007 - AI and chatting
author: Nick Larsen
categories: streams
date: 2019-01-30 16:55:33
youtube_url: https://www.youtube.com/watch?v=Ep9ec1jHkNs
youtube_embed: https://www.youtube.com/embed/Ep9ec1jHkNs
---

Today we went over some things I took care of off stream, namely refocusing the code to more closely match what you see when you visit wikipedia.  This big change came about when I showed some friends what we have been working on and their first complaint was "wow, that's really hard to follow".  Thanks y'all.  That's double speak because at the end of the day they were right, it was hard to follow and most notably I left out the closed set, which is what makes BFS act like BFS and not search the same node multiple times.

On that note turns out we have a bug with the `OpenSet` where we still are exploring the same nodes multiple times.  That basically means the set isn't respecting the set property.  To make this easier to see, we added a test project and change the fast.search project into a class lib instead.  Overall the code is cleaner and easier to run small little segments of code at a time.

Thanks to everyone who joined and I look forward to seeing you all tomorrow!
