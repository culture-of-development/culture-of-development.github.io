---
title: 'js13k competition practice, part 12 of 12: finishing the build pipeline and compressing the audio (day 39)'
author: Nick Larsen
categories: streams
youtube_url: 'https://www.youtube.com/watch?v=iS8tTMI2eEk'
youtube_embed: 'https://www.youtube.com/embed/iS8tTMI2eEk'
date: 2019-05-10 09:20:01
---

I saved the most important requirement for this entire project for last, compressing everything into 13 kb.  That's why it's called JS13K after all.  I knew this was going to be difficult because of the audio.  I had looked at how many other competitors had implemented audio in the past and frankly it's still really complicated to me.  I decided to take the bail route and just record some sound effects when we reached that step to save some time.  

For the first part of today I fixed up the build script that combines all the files into the correct folder, zips them up and then spits out the size.  This was a bit frustrating honestly because I rarely use the commandline and basically everything I expected to work did not succeed on the first try.  After looking at tons of documentation, Stack Overflow questions and a little help from chat, I finally got it performing all the necessary steps.  Where are we at?  Something like 230 kb.  A good bit off.

Next I took the bail route again on the audio.  I knew we didn't need the fully sampled audio, so I looked to audio compressors I could find online with the idea of compressing it until I couldn't stand the quality loss.  This was super powerful; after converting all my audio to 16k sample rate instead of the original 192k sample rate the size was down to just under 30 kb total.  Still not quite there, but at this point I'm out of time and this was as good as I was going to get.

At the end of the video I take some time to recount all the things I learned throughout this project as well as a bunch of stuff I want to try out before the next competition starts in August!  Look for a blog post on that soon.