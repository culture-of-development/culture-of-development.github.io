---
title: 'S0037 - js13k competition practice, part 10 of 12: building a world designer'
author: Nick Larsen
categories: streams
youtube_url: 'https://www.youtube.com/watch?v=Cap0xSfqWIQ'
youtube_embed: 'https://www.youtube.com/embed/Cap0xSfqWIQ'
date: 2019-05-08 08:52:18
---


If we're going to build a large and engaging world, we need some sort of level designer.  This of course all has to fit in a reasonably small size as well so I figured I would start by looking at what other people have done in the past.  Many of them use images where each pixel's color represents a particular set of features about that location.  Sounds like a good idea, so off we go building out a nice big office building in Paint.NET.

The fun really starts when we try to get this image data in the browser.  Thanks to roberttables for numerous ideas today.  At the very we get the data loaded up and verify that all of the values we have loaded are known values, then I have a hard stop for other obligations so we'll have to delay the actual building of the new game world until tomorrow.

After today, the level designer definitely feels like one of the most important parts of the game design process.  The ability to quickly prototype ideas is largely based on how quickly you can build out a level and play through them.  Prior to today the only means we had to build out levels was a manual process of deciding exactly where everything should go, having a constant world size and moving things around was very painstaking.  This dramatically improved my overall experience with that aspect of the game and I want to spend some more time thinking about how to do this better for future projects.