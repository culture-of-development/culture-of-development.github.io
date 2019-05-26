---
title: 'S0029 - js13k competition practice, part 2 of 12: a movable player character'
author: Nick Larsen
categories: streams
youtube_url: 'https://www.youtube.com/watch?v=sGjJD4E-yQo'
youtube_embed: 'https://www.youtube.com/embed/sGjJD4E-yQo'
date: 2019-04-11 10:06:23
---

Now that we have a simple grid world, it's time to make some sort of interaction so we can start playing with it and feel out what works and what doesn't with our ideas so far.  We make a simple way of interacting with the game by moving a purple box around the grid world we have created.  Since this didn't take too long we also added some items to the world and wrote enough code to place them where we want them and give them custom attributes.

In addition to that, this competition is about building assets that total to less than 13 kb, so we constructed a rudimentary build pipeline that collects the assets, passes it through [uglify-es](https://www.npmjs.com/package/uglify-es), zips it all up and reports the total size.  We're barely at 2 kb so far!