---
title: S0043 - AI & Chatting - Decision Trees code
author: Nick Larsen
categories: streams
youtube_url: 'https://www.youtube.com/watch?v=jMXf-VFwPJI'
youtube_embed: 'https://www.youtube.com/embed/jMXf-VFwPJI'
date: 2019-05-22 09:12:28
---

For the next couple weeks we get to work on stuff for my primary job!  Essentially we have come up with a new model for estimating the click likelihood of job ad on Stack Overflow.  The model was trained up using the [XGBoost](https://xgboost.readthedocs.io/en/latest/) R package.  It is a boosting method over the top of decision trees.  On the prediction side this model acts exactly like a random forest, i.e. a bunch of decision trees that all vote on the correct answer, which in this case is whether or not the person will click on this job.  The only difference is that instead of doing it as a classification problem, it instead works more like a regression problem, calculating a probability as the output instead of a binary yes/no answer.

Initially we decided to try making an API in R to support this model which saves us the trouble of converting the model to another language and dealing with bugs along the way.  After some quick testing we realized that was going to be wayyyyyy to slow, like 10-100 ms _per job prediction_!  There are thousands of jobs that have to be predicted on each request, so this is definitely not going to work.

Our task is this:

- perform 5,000 prections
- complete all predictions in roughly 50 ms
- do all these computations on a single processor core
- use a reasonable amount of memory

The last two requirements are based off the fact that this is going to be running on shared hardware and screwing this up can have serious consequences for the entire network.  If you're doing the math in your head, this is about a 2,500 times speed up we need to achieve, so this ought to be fun.

