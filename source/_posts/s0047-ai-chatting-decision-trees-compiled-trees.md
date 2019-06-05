---
title: 'S0047 - AI & Chatting - Decision trees: compiled trees'
author: Nick Larsen
categories: streams
youtube_url: 'https://www.youtube.com/watch?v=5KoSbsvPjwQ'
youtube_embed: 'https://www.youtube.com/embed/5KoSbsvPjwQ'
date: 2019-06-04 16:43:39
---

Well after all that work on optimizing for fewest pages touched, turns out it didn't help any noticable amount.  Completely... deflating.

Of course we're not out of ideas yet, far from it in fact.  TheHugeDahl was extremely thoughtful during the last stream and noted that we could make some changes to the organization of the trees.  One improvement in particular was moving the leaf values up to the parents.  If we can move the leaves to the parents we save a branch of every single tree which should add up to some savings.  But... why not take it one step further.  Lets try code generating functions for each tree and just calling those functions directly.

Ideally this would have a lot of positive effects:

- it would eliminate the heavyweight trees altogether
- it would eliminate a lot of data cache contention by moving the trees to instruction cache
- and of course we could eliminate those last branches in each path

One caveat, I have never written code gen using .NET before, so we're going to have to figure that out and luckily there's a library called [sigil](https://github.com/kevin-montrose/Sigil) to help us out.
