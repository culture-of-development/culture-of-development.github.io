---
title: S0015 - Implementing a demo front end for the finding directions problem
author: Nick Larsen
categories: streams
date: 2019-02-15 13:56:26
youtube_url: https://www.youtube.com/watch?v=q6dqx_PQ91Y
youtube_embed: https://www.youtube.com/embed/q6dqx_PQ91Y
---



Today we built out a webapi app the load up the model and let us get direction instantly on a Google maps interface.  Using this we did some really basic profiling and found that the majority of the time is spent finding the closest node to the click location and not the actual search itself.  I guess we know what to fix before we ramp up the problem size again.