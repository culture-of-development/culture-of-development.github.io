---
title: Implementing the finding directions problem (day 12)
author: Nick Larsen
categories: streams
date: 2019-02-08 21:37:42
youtube_url: https://www.youtube.com/watch?v=oKKr8syPjTQ
youtube_embed: https://www.youtube.com/embed/oKKr8syPjTQ
---

Yesterday we abstracted away the concept of a problem to be solved by a search algorithm and today I decided we should go ahead and test our theory that it was successful by implementing a second problem.  I decided to choose the problem of finding driving directions because it's a hard problem with a sufficiently large amount of states.  It's also just kinda cool to think we can implement the same kind of algorithm used by Google Maps and at the same scale.

We spent a lot of time designing the problem definition first and working our way backward to the actual data.  Check out the video for a short explanation of exactly why we designed things that way instead of trying to find a bunch of data first and working forward to the actual problem definition from that (spoiler alert: the code came out really clean and readable).

As time was running out I tried to get a test up and running so I grabbed the classic Romania map problem from [_the_ AI book](https://amzn.to/2GiSiPg).  I ran out of time before setting up a test for this, but we'll get to that next week.  In the meantime, please check out [the code](https://github.com/culture-of-development/fast) and we'll see you all next week!