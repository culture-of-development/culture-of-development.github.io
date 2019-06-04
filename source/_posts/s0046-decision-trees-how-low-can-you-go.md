---
title: 'S0046 - Decision trees: how low can you go?'
author: Nick Larsen
categories: streams
youtube_url: 'https://www.youtube.com/watch?v=Ged02QfW9uo'
youtube_embed: 'https://www.youtube.com/embed/Ged02QfW9uo'
date: 2019-06-04 08:31:06
---


Last week we used performance tooling like dotTrace and Intel Vtune to show that the majority of the time our code takes is in fact memory loading issues.  Since memory is broken into pages, we take a look at exploring how many pages are being traversed while loading the features and see for a weighted pass through any tree, we're touching roughly 4 pages of memory.  If we can lower this by even 50%, then this should make a dent in overall runtime.

Our approach for this episode is to build a greedy optimization tool to see how low we can get the average number of features memory pages per tree down to.  At the end we make some predictions, all of which turn out to be wayyyyy off.  Off stream I made some minor adjustments to the evaluation function and overall we were able to find an ordering of features which reduces the average number of pages hit from roughly 4.3 per sample per tree to around 2.8 per sample per tree.  Since there are 1,000 trees and 5,000 samples, that's roughly a ton of work saved.  At least that's the expectation.
