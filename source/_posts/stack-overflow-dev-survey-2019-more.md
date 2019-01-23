---
title: Stack Overflow Dev Survey 2019 + more
author: Nick Larsen
categories: streams
date: 2019-01-23 14:23:13
youtube_url: https://www.youtube.com/watch?v=F_Dc-LUnmg8
youtube_embed: https://www.youtube.com/embed/F_Dc-LUnmg8
---

Today we started off with me filling out the 2019 Stack Overflow Developer Survey and talking a little bit about myself when answering the questions.  Not all of them make perfect sense and there was a little ambiguity which was fun, and I got talked a little about some of my crazy artificial intelligence predictions for the future.

After that we moved on to a little infrastructure work for some search algorithms, in particular implementing the [N-Puzzle](https://en.wikipedia.org/wiki/15_puzzle), that we're going to use as our test bed to learn about some more advanced search techniques.

In the closing few minutes I explain the long term goal, which is to reframe the matrix multiplication problem from last week as a search problem in order to write a program that can find the optimal way to do dense matrix multiplication and beat the speed we initially set as a target by doing a 2048x2048 multiplication in python that finishes in like 25 ms.  The python implementation is delegating out to MKL on my machine which means if we can beat this, we beat the implementation by the people who build the chips, and that sounds like a lot of fun to me.

Thanks again to everyone who joined us today, see you all soon!

