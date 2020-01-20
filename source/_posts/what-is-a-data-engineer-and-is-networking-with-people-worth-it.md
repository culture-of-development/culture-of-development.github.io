---
title: What is a data engineer and is networking with people worth it?
author: Nick Larsen
categories: mentorship
date: 2020-01-11 10:28:12
---


## Dear Nick

What do you do as a data engineer, or what does your day to day work look like (perhaps a time percentage break down of the types of tasks you do)? That ones mostly just for my curiosity though!

Also, if you don't mind sharing; have there been any challenging aspects that have come up in your programming jobs that you weren't expecting? I'm trying to cultivate realistic expectations. 

Any thoughts on networking and it's relative importance? I'm trying to attend some meetups in my area, but it's a bit difficult with my job hours. I'm also interested in relocating to another part of the country, so I'm not sure how much time I should really spend on it unless it is key. Maybe online networking? I'm not really sure what that looks like exactly though, hm. 

Best,

Cameron

---

## Dear Cameron

### Being a data (ML) engineer

Being a data engineer is really fun.  My super high level explanation for it that it's like DevOps for data scientists; essentially my job is to make their jobs as easy as possible.  A lot of the time I gather data, internal data, external data, APIs, web scraping, and sometimes directly asking people questions and recording their answers.  Once I get the data, I clean the data as much as possible which is specific to the use case at hand.  Sometimes this means transforming things to be consistent, like converting all phone numbers to the same format or replacing shorthand words in text with their more common ways of saying things (tag replacement in general is a big one at my current job), or extracting meaningful metadata to provide additional context, like passing IP addresses through a location filter to say this request came from this country, etc or more interesting transformations like filling in missing data when possible or throwing away less useful data.  

After that I make this data accessible.  Sometimes it's dumping it in a database, sometimes it's generating csvs or running periodic reports that generate visuals for dashboards.  And after that I deliver it to the people who need it.  Sometimes this is building libraries to access it from whatever data store it's in for whatever language the person likes to use, etc.  Most of my work is in accessibility because making terabytes of data easy to search through is hard.  There's lots of performance considerations and storage considerations and you have to be intimately familiar with all the bells and whistles available to you to keep things running smoothly since your dataset just keeps getting bigger every day.

If that wasn't fun enough, once the data scientist or other ML engineer comes up with a model (sometimes my own models) then it's my job to make that model work in production.  This usually means building or modifying a datastore to hold preprocessed data about people we want to predict stuff for, then scaling that up to work with all the users of our site, not just the handful we used to train the model.  This also means a lot of integration work with other teams to actually use the model we've built and a lot more work around collecting the results of people using the models so we can accurately evaluate whether or not we have made the improvement we were after.  And that's the gist of it.  I also spend a fair amount of my time documenting processes and assisting various teams with validating (or not) their ideas for product changes by looking at what data we have to analyze.

### Unexpected challenges

There have been enormous unexpected challenges, way too many to enumerate, but to narrow it down and focus on one, it would be building the right things.  Or rather, not realizing when you are not building the right things.  There are few things in life more demoralizing and demotivating than spending 6 months or a year building something and then nobody uses it, realizing that all that effort was wasted.  The only way I can reliably tell you to not do that is to put yourself as close to your users as possible, even be one of your own users as much as possible, but still listen to what their pain points are.  There's a real pleasure in building something that makes someone else's life easier, make sure you can see that happen, try your best to experience it yourself and give gratitude to people who do the same for you at every opportunity.

In all likelihood, nearly everything you build today will be obsolete in 10 years.  Other people will come along and do something completely better.  Keep building new stuff, even when the thing you have built is awesome, keep building new stuff you're excited about.

### Networking

Networking is important.  For me it's mostly about keeping up with people who are working on interesting things.  A follow on twitter or a linked in connection is often good enough a way to keep people informed about what you are doing and for you to keep up with what they are doing.  Yes network in your area now, you never know who's going to be there because people travel for those things all the time.  I can't tell you how many times I've been visiting family in random parts of the US and needed something to do one night so I just found a local meetup.

When you meet people, first always ask them something specific about themselves, nothing general.  After that, I strongly encourage you to leave them with a story, something short and important to you at that moment.  People remember stories.  Tell them about that one problem you ran into and how you solved it, or this crazy event where you had a realization of something.  Tell them about good stories you have heard from other people, do whatever really.  Even if they forget that story 10 minutes after you leave them, as long as you have their attention for those 2 minutes, they will remember it when you bring it up years later. 

I've been working on this response bit by bit for a few days because of some enormous distractions, so I apologize for the delay but do please keep the questions coming.

Thanks,

Nick