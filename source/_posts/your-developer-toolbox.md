---
title: "Your Developer Toolbox"
author: Nick Larsen
date: 2013-12-27 10:33
categories: blog
---
As you probably know, when we're interviewing someone, we're looking for people who are [smart and gets things done](http://www.joelonsoftware.com/articles/fog0000000073.html).  Most interviews that end with a "no hire" fail both of those checks.  Don't despair however, it's not that they are bad developers, they just need more time to fill up their developer toolbox.  Having a well stocked developer toolbox typically solves both of these problems.

So what's this developer toolbox you ask?  Your developer toolbox is the set of tools you can reach for when solving a problem.  The languages you know, the algorithms you can readily implement, the frameworks you are familiar with, the paradigms you can map problems to, how you look for solutions to the problems you face and even the people you talk shop with.

![Toolbox, not quite the same, but you get it, right?](http://upload.wikimedia.org/wikipedia/commons/thumb/f/f4/20060513_toolbox.jpg/640px-20060513_toolbox.jpg)

The image above was lifted from [wikipedia](http://commons.wikimedia.org/wiki/File:20060513_toolbox.jpg) and it's probably the only image of a non red toolbox on the internet.

### Not too much, not too little, but just right

Interviews tend to spiral downward when someone has either an underdeveloped or overdeveloped toolbox to pull from.  In both cases, gets things done tends to be an issue.  

Underdeveloped toolboxes lack clear evidence of the smart part.  Underdeveloped toolboxes are easy to call out because the interviewee simply doesn't know enough about a technology or pattern to make the decision to use it or not as a means for solving the problem.  They skip using a tool they know of because they aren't sure if it will make the solution easier, or they skip using the tool because they don't know if it will improve performance.  And obviously everyone skips using the tools they don't know about.

Overdeveloped toolboxes are when a person knows a small set of tools really well.  Like... really, really well.  Need to tell if a block of text contains an email address?  They know this regex off the top of their head.  Need to tell if a number is divisible by 3?  Convert number to string to and match against this regex [be damned that you could just actually divide by 3! this regex is much more interesting!].  Need to parse html?  There's a regex for your case.  Seriously.  And they are right, their solution will work for whatever problem you throw at them.  Unfortunately, these people tend not to stray to find better tools for the job and are generally only interested in  being the guy who has to maintain the code these people wrote is excruciating and demotivating work.

So how do you get it just right?  Simple, learn what the tools are intended to be used for, and stick to using them for those things.  You really only stray from that dogma when you can't find something that fits your situation exactly, and you choose the best thing you can find.

### Filling up your developer toolbox

A well stocked developer toolbox goes a long way to at least making you come off as someone who is smart and gets things done.  Although your toolbox includes sites like [Stack Overflow](http://stackoverflow.com/) and all the documentation on the internet, the things you know without having to look them up are what make you fast.  

What you fill your developer toolbox up with is up to you, but you should base it on your interests.  If you love being a web developer, you'll find a lot of technologies mandatory which a game developer will not.  There are, however, some basics which are (almost entirely) cross cutting and you should know them thoroughly.

1. Analysis of algorithms: looking at a piece of code, estimate it's run time and memory needs compared to other algorithms for a given input size
2. Sorting: the various sorting methods and in particular their run time for different input sizes
3. Searching: a handful of various binary search trees, hash structures and string processing... and in particular their run time for different input sizes

As you can see, analysis of algorithms is pretty important.  **A well developed ability to estimate the run time of an algorithm is one of the primary tools for making smart decisions.**  Understanding the run time at various input sizes gives you the ability to make better choices for algorithm performance up front when thinking about your solution rather than just implementing the most naive possible solution.  This saves you time, particularly in projects when relying on the built in tools no longer get the job done (e.g. queue implementations tend to optimize for either push or pop, and practically no languages include good implementations of both).

Searching and sorting are simply fundamental.  Many practical problems decompose to being exactly a searching problem, a sorting problem or some combination of the two.  For many of these solutions, the computational time spent doing these tasks can easily overwhelm everything else.  Despite often being a single line in your pseudocode, your implementation choice can often make a world of difference in the quality and performance of your solution.  On multiple occasions, two interviewees have come up with the same solution pseudocode, but after implementing it, only one person moves on.

Beyond this you want a wide breadth of tools from many different topics and a strong concentration in one area, preferably the area you are most interested in.  It's impossible to overstate the important of having a wide breadth of tools.  The breadth of your toolbox helps you come up with creative solutions to problems and can often help you find places to look for solutions you wouldn't have otherwise known of.

### Learn the important parts

When you are studying these algorithms and data structures, there are 2 things you need to make sure you commit to memory.  First, you need to understand the various run times for average case and worst case for _each operation_ in the data structure, and second, you need to actually be able to explain how an implementation would work in order to achieve that run time for each operation.  Thinking a faster solution exists isn't particularly useful if you cannot implement it.  Often times there are multiple ways to implement a data structure so that it has the exact same run time for each operation but the solutions have different memory requirements and readability concerns.  Knowing more than one implementation can often make your code even faster to implement (or more readable) as long as you know the typical input size up front.

Knowing the run times should be enough to make you understand what the data structure or algorithm are useful for, but it still helps to explicitly come up with uses when you are studying.  For any problem, you need to be able to say why you chose the algorithm and why you chose the particular implementation.

### This all seems pretty academic

![learning, and shit](http://upload.wikimedia.org/wikipedia/commons/thumb/e/e7/Academic_Quadrangle.jpg/640px-Academic_Quadrangle.jpg)

The above image was lifted from [wikipedia](http://commons.wikimedia.org/wiki/File:Academic_Quadrangle.jpg).

I know; practicing programming by studying and not just implementing whatever ideas come to mind?  How blasphemous!  Remember, the goal here is to make you not just faster (gets things done), but also to make you smarter.

Having an idea of what smarter means and simple steps for how to achieve it will hopefully give you a place to start thinking about how to improve yourself.  

You save time two ways.  Practicing implementations for and with various tools helps you get better at using them by learning their APIs and giving you experience to reflect on when coming up with new solutions.  You are almost certainly doing this already however.  You also save time by virtue of being smarter.  Since you have a wide range of tools to pull from, you can find something that just right for this problem much faster and you can often skip naive solutions which makes your solution sufficient for much larger input or load as the problem scales.

Discussion on [Hacker News](https://news.ycombinator.com/item?id=6984951).