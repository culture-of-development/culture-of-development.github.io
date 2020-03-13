---
title: When can you say you're fluent in a language?
author: Nick Larsen
categories: mentorship
date: 2020-02-17 21:50:11
---


## Dear Nick

When can you say you're fluent in a language? I've been using and learning Python for a couple months, and have been doing some data-analysis type projects, but I'm not sure if that really counts as fluent. I've been learning SQL for an even shorter time, so perhaps I should use wording more along the lines of "Experience using SQL" instead of fluent. I suppose I'm worrying about overstating my skills in a deceptive manner.

I have some past research experience and have been working on data collection and infrastructure at my current position, but in both instances, I've worked with smaller datasets in more limited ways, which I also don't want to misrepresent. I've been working on projects through some online courses though; would those be good to mention in a resume? Or perhaps it would be better to put them up on Github, though I do not know if interviewers for Data Analyst positions expect/want to look at projects like an interviewer for a developer position would.

I've read through your blog post on cover letters and interviews, which has been very informative, and am reading from some other resources too. Do you know if Data Analyst/Scientists interviews go into technical questions to the same extent as developer interviews? From reading and watching some online videos, my impression is that there may be some technical questions, but not to quite the same level- not so much producing code for practice problems.

I welcome any other advice or input of course! Thank you so much as always for your help; I've been really blown away at how many incredibly helpful and supportive people I've been able to talk to just through learning some coding. Hopefully that holds true when I start working more with people in the field too.

Thanks,
Cameron

---

## Dear Cameron

That's a great question.  Proficiency is entirely relative.  No one, and I mean no one, knows everything about a language, not even the people who work on the actual specifications.  They are just too big, there are just too many edge cases, etc.  But if you decide that the person who knows the most about a language is a 10 and someone who knows the least is a 0, then most people who use a language professionally fall below 3 for most languages, and are considered totally proficient in their work.  Once you get familiar with a set of tools you can usually use them to solve most problems just fine.  There are only some cases where some esoteric language knowledge is necessary (like for instance how structs and classes are handled differently in most garbage collected languages) that can make the difference for some particular problem.  But usually you can get a lot done with just a little knowledge.

When I say it's relative, what I mean is that you will be judged by the person who is evaluating you and they almost always consider themselves to be the bar, and unfortunately, they are going to be very familiar with the problem at hand but you are not.  In an interview setting you'll be judged on whether or not you knew where the function you needed was, and if you don't know how to use it, how easy it was for you to find the information to use it.  Your best bet is to look up the common tools used in interviews and be prepared to use them.  Anything related to string parsing (including parsing numbers and splitting), and every kind of collection built into the common framework for your language.  Learn them all, and be handy with using them, there is little else you'll be tested over early in your career.

The easiest way to skirt the question of expertise on your resume is to just not list your declared proficiency.  Don't make it an issue by simply not declaring it.  Just say "skills: python, whatever, something else, etc".  You marking your skills in python as proficient isn't changing anyone's mind about whether or not you are the right person for the job when you're looking for your first job, and they will estimate your skills during the interview process.

Yes list any moocs you have done on your resume, with dates.  They don't need to be long or have descriptions but they set up a timeline that shows you're learning, how long you've been learning and any experience before you have experience helps.

There are really 2 kinds of data scientist interviews I have performed, the first is the "full stack" data scientist role.  This normally happens when you're one of the first data scientists at a company or they when they have some sort of culture that only hires generalists.  If you have to set up all the infrastructure at a company, expect to land in this kind of interview.  The other kind is a more analysis focused interview.  In both cases however, data science is a computational role, so you'll be expected to know how to code.  Expect to have one interview on data wrangling (loading up some raw data, cleaning it and getting it ready for learning, maybe with some light modeling), one that tests your stats knowledge and probably a generalist problem solving interview or a take home test (usually for a data scientist, not so much analysts).
