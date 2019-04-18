---
title: 'js13k competition practice, part 4 of 12: adding sound effects to interactions (day 31)'
author: Nick Larsen
categories: streams
youtube_url: 'https://www.youtube.com/watch?v=qSy2jUtZjcI'
youtube_embed: 'https://www.youtube.com/embed/qSy2jUtZjcI'
date: 2019-04-16 10:27:56
---

This stream was at an unusual time because I unexpectedly have to leave this weekend and don't want to get too far behind on the one month aspect of this practice.

Today we worked exclusively on adding sound effects to interactions using the [web audio api](https://developer.mozilla.org/en-US/docs/Web/API/Web_Audio_API).  There are essentially two ways to incorporate audio, either from a buffer of recorded audio data, or by manipulating a source wave from an oscillator.  Recorded audio gets to be quite large depending on the sample rate used to record the audio (recording 1s of uncompressed audio at 44k samples/sec eats up 44 kb) so the eventual audio we use will almost certainly be generated from an oscillator.

In looking at how js13k projects incorporate interesting audio from past years, I've noticed that many of them generate sound, but record it to a buffer, and then play the buffer on demand instead of doing all the calculations to generate the audio waves repeatedly.  This helps a lot with performance and also simplifies the playing code a fair amount.  Since we're going to be playing from buffers anyway later on, I decided for this first iteration to just record some audio and then load it up in the browser, focusing mostly on getting the sound effects to trigger at the right times.

Today was a lot of fun and [theMichaelJolley](https://www.twitch.tv/themichaeljolley) showed up to help me work through some ideas and spit ball what we should do next.

