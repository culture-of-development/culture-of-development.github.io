---
title: 'S0036 - js13k competition practice, part 9 of 12: lighting up the world'
author: Nick Larsen
categories: streams
youtube_url: 'https://www.youtube.com/watch?v=tmkBz8HAW5U'
youtube_embed: 'https://www.youtube.com/embed/tmkBz8HAW5U'
date: 2019-05-01 08:16:06
---

Today's big goal was to get the lighting done.  The last critical element of the story is that it's dark in this office and you have to look around.  Around the windows there should be some dim, ambient lighting and there is a flashlight you can find that will make the area around you light up as well.  And don't forget that light should respect walls as well.  Prior to today, I had absolutely no idea how I was going to achieve any of these goals.

First things first, just make it dark.  First idea was to just put a giant div over the whole thing and have the background be all black, except make it transparent where there was light.  Unfortunately I could not figure out how to do this using multiple backgrounds with CSS, and I didn't want to have one super huge mega image covering the whole board because I felt like that would cause significant resource issues.  The image alone would have been over 10 MB and it would have been manually redrawn on nearly every move.

My second thought was to just have set levels of light that were available, and do a cell by cell lighting.  I decided to roll with it.  We ran into some CSS issues with the walls, which were set directly on the cell itself.  Instead I moved the walls to their own layer, nested inside of each cell and appended an additional layer for the lights on each cell.  This appears to work and boom, we have some darkness on top of everything.

Adding in the ambient lighting happened first, which was not particularly difficult, we just defined some cells as windows and then did a once over pass after building the grid to set the ambient light level and after fudging around with the levels a bit, I landed on something that sort of met my expectation.

The flashlight was a bit more challenging.  The interaction system already had a function that could handle player movement, and took in the previous location and the new location.  I thought to myself, simple enough, we'll just reset the old location lighting, then we'll set the new location lighting.  Yea... not so easy. :)  Watch the video to see how we work through that one.