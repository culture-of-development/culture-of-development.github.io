---
title: 'Building a slack bot, part 1: I know nothing about this (day 22)'
author: Nick Larsen
categories: streams
youtube_url: 'https://www.youtube.com/watch?v=OUFMi7TZ55o'
youtube_embed: 'https://www.youtube.com/embed/OUFMi7TZ55o'
date: 2019-03-07 09:26:02
---

At Stack Overflow we have a few bots in our bespoke chat app that really make it a lot of fun to use.  Due to the pressures of not maintaining something that isn't our bread and butter (i.e. Q&A), we've slowly but surely been moving everything over to Slack.  One of the tools that's the most fun is something called the pic bot.  When you reply to an image with the phrase "needz moar {something}" and then replace the something with the thing you want, it modifies the image to add some extra flare to it which normally results in hilarity and a good laugh for everyone.

![boring screenshot](/img/slack-bot-before.png)

```
needz moar jorts
```

![amazing screenshot](/img/slack-bot-after.jpeg)

And that's essentially the effect we're after.

Starting today, I know nothing about how to build Slack bots, watch me get up to speed and get a bot running that actually responds to messages.