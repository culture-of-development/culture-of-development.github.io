---
title: "Draft your code"
author: Nick Larsen
date: 2013-06-19 15:42
categories: blog
---

The essence of craftsmanship is skill.  Skill is developed by practicing a task until it is understood as completely as possible.  Ergo, practice your craft.  The act of practicing a task can often be done in many ways, and the methods are comparable by how quickly they improve your understanding of the task.  A craftsman opts to practice in the way that most improves their understanding of the their task.  If you are a software craftsman, _draft your code_.

## The departure of craftsmanship.

Around the time I entered middle school, computers had gotten into just about every class room and many people had one at home.  We no longer wrote our book reports and three point essays by hand, we typed them up on the computer and printed them out before we turned them in.  I remember prior to this repeatedly being told that we should write rough drafts and then refine them.  In fact, I remember some of my earlier assignments even required me to turn in those rough drafts along side the final paper.  Somewhere in the first few years of writing papers on the computer, that requirement went out the window.  Sure you can just highlight and delete the parts you want to edit out or just add a few modifiers by moving the cursor to the appropriate position to edit it on the fly, but somewhere in there the concept of drafting your thoughts was lost.

Back in those days, I was one of the proprietors of this new way of thinking.  I would write the paper all the way through to the end, hand in the rough draft, then my final paper was just copying and pasting sentences in a different order.  I was simply changing it because I was told to, not because I thought it was better how I changed it; I wasn't even thinking about that.  In the end it turns out I am not a craftsman of the written word.  I leave this post as evidence.

## "The best code in the word is meaningless if nobody knows about your product."
 
Back in those days, I had already started my career as a hacker.  Unfortunately for me, and the first few people I ever shared my code with, I applied my lack of craftsmanship in writing book reports to my hobby of writing code, and I basically refused to change it once it worked.  I also remember reading every available article on game development and repeatedly seeing the mantra _once it works, the code is right_.  No one expects your code to look good when you're 11 and just learning to program, but it's a fact that you apply lessons from domains you are more familiar with to new domains as you encounter them.  This is why you say "hello" when introduced to new people instead of punching them in the gut.  Perhaps it's no coincidence that hello world is such a popular program to implement when learning new languages.

The quote in the title of this section is from [Ian Landsman](http://www.ianlandsman.com/2006/10/06/10-tips-for-moving-from-programmer-to-entrepreneur) back in 2006 and in context of entrepreneurship it is a great quote.  However, a context shift has occurred through the repeater of the internet and shortening via Twitter over the years reducing this to the more common expression "the code doesn't matter as much as the product".  This adjusted and context-less phrase is often used as a defense for neglecting code craftsmanship in favor of product thought.  This is completely analogous to my youthful defense for not writing drafts of my book reports because I could just edit it on the screen.  In both cases, it's craftsmanship that's left behind.

## There are many so many resources out there.

There are many more software developers out there today then there were when I started programming 20 years ago.  Many of them are entering the field at a later age and are writing code as a bad as I was back then.  Much like I read everything I could on game development back then, people are trying to to connect to more skilled developers through twitter, following blogs and picking up books.  It's great that all this stuff is available today, I only imagine where I would be today if I had picked up better habits earlier on.  But it occurred to me recently that people naturally trust the ideas of other people they feel are more skilled than them, almost blindly.  I did it back then, and I hear it in much louder voices today.  When an older developer takes a quote out of context to discourage the importance of craftsmanship, it makes me want to punch them in the gut.  When a newer developer uses the same quote to deny the importance of craftsmanship, it makes me want to cry. 

Luckily today there is a whole library of books, blog posts, youtube videos, presentation slides and other stuff on code craftsmanship, so it's much easier to right the misunderstanding in the flexible ideas in newer developers minds.  Occasionally this wealth of knowledge gets the best of people, overwhelming them with too many concepts which they do not completely understand.  Most often you can tell when someone is overwhelmed because they will almost tell you outright that they have formed this idea of perfect code in their head and as a result.  The result of this is usually writers block, because they want to write this perfect code, but they don't understand the concepts enough to know if they are even writing good code when in fact all they really need to be writing is working that meets spec<sup>1</sup>.

## What about refactoring?

Refactoring is one form of code drafting.  Refactoring normally means, we know there is some kind of problem with how the code is now and how we need to the code needs to interact with something else, so let's adjust it to meet those requirements as well.  When I say _draft your code_, I mean write that shit 5 more times immediately after you implement it for the first time.  5 is an arbitrary number; what I really mean is write it enough times to understand it thoroughly.  Verify that it works after each implementation.  Typically I limit the scope of this advice to algorithm and pattern implementation, no higher.  Each time try using different abstractions, write down the memory usage, runtime expectation, then consider the integration points for the rest of your system and the overall readability.

I also encourage you to scratch rewrite the code at least once.  It's easy to fall into the trap of thinking you can just move things around and change the name of your functions to something more readable, but you're likely to gain a false sense of confidence with this new algorithm or abstraction simply because you were able to make it work in this one system this one time.  For any sufficiently complex algorithm or pattern, you'll often find the one way it worked in that one system really _was_ only good for that one system.  When learning new concepts, the craftsman strives for something better; they strive to identify and abstract the core components.

## An example.

Recently I started watching the videos for the [Natural Language Processing](https://class.coursera.org/nlangp-001/class/index) course on Coursera.  In the first homework you build a language model to identify gene names in a blob of text.  As per normal, the first time I implement a new algorithm, I create a new project in my dev environment and give it a shot.  The first part of the homework was to calculate emission probabilities for a hidden markov model (HMM).  After a couple of hours, I had something that was able to generate the correct solutions so I moved on to part two which was to select word tags trigram probabilities from the same HMM using the Viterbi algorithm.  I hacked this together and then modified it once more for the last part of the homework which was to support different classes of rare words.  All in all this homework took me about 5 hours to get through.

Hopefully you're like me and you're at most vaguely familiar with the concepts I just listed.  I was able to solve the problem and I was pretty pumped to see the results match the test data.  In the past I would have turned it in and gone to bed.  Instead, I immediately deleted the project I was working in and started a new one.  Then I did the homework a second time.  The first time through, the whole thing was implemented as a small set of functions consisting of about 150-200 lines.  I was doing this in an object oriented language and I didn't create a single class.  This is pretty typical of a first implementation.  I understood the mathematical concepts of the models before hand, but having never _implemented_ them, I had no idea what the best abstractions were.  Now I had a better idea, so it was time to find a better way to write it.

For the next implementation, I considered how this might be used in an actual application.  Essentially my application would have sentences broken down into word collections and I would want to pass each word collection to a function that returns a collection of equal length with the tags assigned to each word.  The core logic was only to do the tagging, so I created an interface with a single function to do the tagging:

    interface ITagger
    {
        string[] GetSentenceTags(string[] sentence);
    }

I wrote all of the input parsing logic in my new project, wrote all the functions that use this interface and made sure it compiled.  Now it was time to implement this function.  I remembered implementing the `ArgMax` function repeatedly, so the first time I ran into it, I knew to pull it out into a function.  I also remembered the whole structure for holding the word counts was deeply nested generics, so I decided extract a `WordInfo` class that also cleaned up the word counting and tag checking code from the first implementation.  I also used a completely new mechanism for storing the back pointers in the Viterbi implementation.  Now this thing was looking a lot better and I had developed some very useful helper classes.  There was a lot more code in this version, but it was much easier to understand and in general I felt like I had identified a couple of useful helper classes.  Additionally, I was able to debug the Viterbi algorithm much easier because I understood what it was doing.

Then I deleted my implementation again.  I left all of the junk code for loading up the input data because it wasn't the core logic I was trying to learn.  This time through I decided to implement the model as it's own concept.  The idea was to make the literature (i.e. the lecture notes) translate as directly as possible to the concepts implemented in the code so that if you needed to understand what was going on, you'd just have to read the lecture notes and names of ideas there would be the names of classes or functions in code.  This turned out to be a great idea because it left only the Viterbi algorithm implemented in the `ITagger` implementation and moved all of the data functions to a separate class.  This really cleaned up the responsibilities in the code as well.  Again I decided to use yet another storage mechanism for the recursive values in the Viterbi algorithm and as a bonus, I only had one off by one bug in the implementation this time despite the changes; it was clear I was really starting to understand it.

Now I was happy with the abstractions and comfortable with implementing the hard parts.  At this point I moved into refactoring mode.  In the first pass I broke apart the remaining multipurpose functions.  In the second pass I renamed everything to be specific as necessary and more closely match the names used in the lecture notes.  At the end I DRYed up some obvious duplication.

## Conclusion.

In the end I spent a full day doing something I could have finished in 5 hours.  Instead of just getting the homework finished, I now have experience writing the actual code to implement this in a real system, implemented the solution in 3 distinct ways and have a clear idea of how the important parts work together because I've stepped through the code debugging them multiple times.  The resulting code is more readable, more testable and DRYer than the original implementation.  I stress however, this code is not perfect.  I'm not even sure what the perfect code would be, but I'm certain the result meets the spec and would be much easier for someone else to understand (or me to understand months from now when it breaks).

You might argue that all of what I have done could also have been achieved through refactoring.  I firmly believe you can be influenced by the existence of a working solution to only make trivial changes, particularly if you are a newer developer with a smaller toolbox.  As soon as you envision a change, use that as your starting point for the next clean slate implementation.  This can be just the inspiration you need to come up with a better ideas for other parts of the solution.  Restated simply again, _draft your code_.

### Make it a life goal  

I didn't realize it until it was pointed out recently, but the idea of drafting has been ironed into my day everyday thinking for some time.  Not just in regards to writing code or writing book reports, but **my entire life and everything I do**.  For my first wedding anniversary, my wife made us picture formed from the words of our wedding vows.  It's a picture of a rocket flying to the moon because I have a huge hobby of making high power rockets and my life goal is to go to the moon.  In the image, the words of my vows make up the rocket and my wife's vows make up the moon.  The background is made up of the words to the song we danced to for our first dance.

![rocket vows](/img/vows.jpg)

If you zoom in on the part surrounded by the red box, the words in my vows read:

> When I realized I had fallen in love with you, I wanted so much for everything to be perfect for you from that day forward for the rest of your life.  I have since learned that perfection is built; it is not something that exists in the beginning when we barely know each other only to be foiled in a single moment, never again to be obtained.

 


---
<sup>1</sup>: Thank you to this [Stack Overflow question](http://stackoverflow.com/questions/16735588/applying-clean-code-and-solid-principle-take-me-so-much-time-normal/16736954) for providing the motivation for this blog post.