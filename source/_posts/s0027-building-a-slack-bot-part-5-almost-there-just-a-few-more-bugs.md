---
title: 'S0027 - Building a slack bot, part 5: almost there, just a few more bugs'
author: Nick Larsen
categories: streams
youtube_url: 'https://www.youtube.com/watch?v=xm80JhsGLmU'
youtube_embed: 'https://www.youtube.com/embed/xm80JhsGLmU'
date: 2019-03-15 09:35:43
---

After some much needed progress yesterday, we realized there were just two major problems to overcome before we could call it an MVP (minimum viable product), just that we need to get all the image manipulation done in memory so we can avoid writing to disk and we need to figure out why the image manipulation pipeline fails the second time you try to run an image through it.

We don't actually solve either of these issues today, but we make a lot of progress on what doesn't work and why.