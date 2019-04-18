---
title: "js13k competition practice, part 5 of 12: adding some emoji graphics, a menu screen and a dialog interface (day 32)"
author: Nick Larsen
categories: streams
youtube_url: 'https://www.youtube.com/watch?v=hE8s-HgMPUY'
youtube_embed: 'https://www.youtube.com/embed/hE8s-HgMPUY'
date: 2019-04-18 10:40:11
---

Today blew my mind!  We started off really easy by swapping out a lot of the color filled blocks that represented items with emoji graphics, largely because emojis are essentially free graphics for your game instead of trying to compress those assets.  We'd like to swap them out for something more consistent across browsers later on, but for now it makes it easier for us to reason about the game interactions as we keep working on them.

The second major thing we wanted to tackle was adding in a "leaderboard" page.  As you play the game multiple times, it should record your score and you can compete with yourself as you play multiple times.  It also helps to have this because we need to force interaction with the page in order to get the sound to start.  For today we left off the leaderboard widget and just added a giant "play new game" button to start a new game.  To support the leaderboard eventually, we also added a move counter to the game page to track how many moves you have made.

Next we added a dialog view.  This will be used to drive the story as we progress through the game and it will act as a tutorial interface at the very beginning.  It was a lot of fun building this out and we made a lot of progress today on making this feel like a fully fleshed out game.

I'd like to give a HUGE shout out to [@ClarkIO](https://www.twitch.tv/clarkio) for the biggest raid I've ever gotten!  This easily put me over the top on my metrics for hitting affiliate and I'm really looking forward to getting that email when I'm back from my trip next week.  Please check out his stream when you have a chance, he's made some great tools for the streamer community and he's a lot of fun to chat with as well!