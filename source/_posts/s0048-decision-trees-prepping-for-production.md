---
title: 'S0048 - Decision trees: prepping for production'
author: Nick Larsen
categories: streams
youtube_url: 'https://www.youtube.com/watch?v='
youtube_embed: 'https://www.youtube.com/embed/'
date: 2019-06-04 17:44:33
---


Right after the stream yesterday I finished up the code to generate the compiled trees and wowee are they fast.  10,000 job/user combinations in ~25 ms.  That's finally within our requirements!  Now it's time to clean this up a bit in preparation for production.

Things we take a look at:

- eliminating essentially all allocations
- reducing memory churn as much as possible
- making a test that more closely mimics production use
- cleaning up the code for readability

I would also like to give a huge thanks to [@csharpfritz](https://www.twitch.tv/csharpfritz) for the amazing 70+ person raid!  This helped me blow through a few achievements and gave me a good audience to recap the last 2 weeks of work on this project.  Also another huge shout out to everyone who participating in the bit war, especially [@theMichaelJolley](https://www.twitch.tv/themichaeljolley) and [@cpayette](https://www.twitch.tv/cpayette)!  Thanks everyone!
