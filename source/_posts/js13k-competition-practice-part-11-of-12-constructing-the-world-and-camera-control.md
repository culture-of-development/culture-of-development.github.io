---
title: 'js13k competition practice, part 11 of 12: constructing the world and camera control (day 38)'
author: Nick Larsen
categories: streams
youtube_url: 'https://www.youtube.com/watch?v=EoXEvQ8rRJY'
youtube_embed: 'https://www.youtube.com/embed/EoXEvQ8rRJY'
date: 2019-05-09 09:08:10
---

The first half of today was all about using the world data we loaded up yesterday and constructing the world from it.  This was almost an hour and a half straight of coding and you should see the expression on my face when it rendered on the first try after fixing one minor typo.

We identified quite a few bugs as we worked after this, e.g. there was some funkyness around walls not being blocking and we planned some game mechanics in the world editor that didn't actually exist in the game... wishful thinking I guess. :)  The world was also HUGE!  Definitely did not fit on the screen so we had to fix up some CSS to even find the player.  There were also some interaction bugs on account of not having exactly one of things like doors.  All doors try to check for a terminal state which clearly isn't correct in an office with lots of doors.

The other major project for today was the camera controller.  Since the world was so big, the camera needed to follow the player in some way so that we could always see the player.  We went with the most simple solution I could think of, just keep the player in the middle of the screen and move the world around the player.  This worked well enough to move on so we wrote down a bunch of quirks that still need to be ironed out and wrapped up.

Only one thing left to do, get this whole thing to cram into 13 kb.