---
title: 'How we sped up random forest processing, lots of common performance patterns'
author: Nick Larsen
categories: blog
date: 2019-06-27 22:43:18
---


In [the previous article](/blog/how-we-sped-up-random-forest-processing-getting-the-lay-of-the-land/) we outlined our problem, the major constraints we must adhere to and wrote a straightforward implementation of a random forest evaluator.  We proved the correctness of the implementation and then we set up a naive benchmark to test how much time it was going to take to run 5,000 samples through 1,000 trees, each of a maximum depth of 5 decision nodes.  This currently runs in about 320 ms and we need it to run in about 25 ms.

As a quick reminder, here is a condensed version of the code from the previous article, ignoring most of the boilerplate:

{% codeblock lang:csharp %}
public sealed class DecisionTree
{
    public class DecisionTreeNode
    {
        public int FeatureIndex { get; set; }
        public double Value { get; set; }
        public int TrueBranch { get; set; }
        public int FalseBranch { get; set; }
    }

    private readonly DecisionTreeNode[] nodes;// set in the constructor

    public double Evaluate(double[] features)
    {
        var node = nodes[0];
        while(node.FeatureIndex != -1) // LeafIndex = -1
        {
            int nodeIndex = features[node.FeatureIndex] < node.Value ? node.TrueBranch : node.FalseBranch;
            node = nodes[nodeIndex];
        }
        return node.Value;
    }
}

public sealed class RandomForest
{
    private readonly DecisionTree[] trees; // set in the constructor

    public double EvaluateProbability(double[] instance)
    {
        var sum = trees.Sum(t => t.Evaluate(instance));
        return 1f / (1f + Math.Exp(-sum)); // conversion to probability
    }
}
{% endcodeblock %}

From a correctness standpoint, this code is just fine; that's what the correctness test is there for after all.  From a performance standpoint, however, we still have a ways to go.  In these 34 lines of code there are an incredible amount of very common performance related issues we can address.  Let's take a look at some now.

### The lambda function tax

Linq is an amazing technology that allows you to write extremely expressive code using a declarative syntax.  Want to sum up the results of calling a function on each and every element of an array?  One liner.  And reads almost like the English sentence I just used to describe it.  It's a wonderful tool.

But it has some drawbacks.  The work-performing argument to nearly every linq operation is a function.  If you're not careful about the functions you use as arguments, you might be leaving a fair amount of performance lying on the floor.  

Usually, as in this example above on line 31, the function argument is passed as a lambda function.  When a lambda function is executed, it runs in the context in which it was declared, meaning it has access to the variables in that scope.  This is how we are able to use the `instance` argument which is a local variable to the `EvaluateProbability` function without declaring the `instance` variable in the lambda itself.

Creating the context for a lambda function is a relatively expensive task.  You won't notice it if you only ever generate a few contexts for a particular lambda, but if you are doing it over and over, the taxes start to add up and in our case we're doing this about 5,000 times per request.  

There are a few ways to solve this problem, but the most straightforward method are to either 1) don't use a lambda function as the delegate parameter to a linq function where it's not necessary, or 2) don't use linq. In this case we do need the context in order to get the `instance` variable, so fix #1 isn't really an option and we should just get rid of our use of linq.

{% codeblock lang:csharp %}
public double EvaluateProbability(double[] instance)
{
    var sum = 0d;
    for(int i = 0; i < trees.Length; i++)
        sum += trees[i].Evaluate(instance);
    return 1f / (1f + Math.Exp(-sum)); // conversion to probability
}
{% endcodeblock %}

Simple enough.  It's a little harder to read, but that's just how it goes sometimes.  Let's see what kind of effect this had on the timing.

> [6/28/2019 12:15:04 AM] Time taken for 5000 evaluations: 267.5595 ms

That's a 16.37% reduction in time; that's nothing to shake a stick at.  Let's be really clear when this kind of optimization matters.  When a user visits the site, we need to perform this calculation for roughly 5,000 jobs, and at peak hours we're going to be handling roughly 450 requests per second.  This code gets called _a lot_.

If this were only happening once per request, we wouldn't worry about this kind of optimization because 800 times just isn't that much.  But when you have code that's being called ~2,000,000 times per second, for hours on end, the lambda tax adds up very fast.

### Arrays of classes

In .NET, there are classes, which are reference types, and there are structs, which are value types.  Classes are usually larger data structures in terms of maximum possible bytes used and they can be null where as structs are usually smaller and they can not be null.  When passing a value type to a function or setting a new variable to a value type, a full copy is made, where as with reference types, only the reference is copied.

{% raw %}
<blockquote class="twitter-tweet" data-lang="en"><p lang="en" dir="ltr">This gif never gets old. <a href="https://t.co/QFz78yi28o">pic.twitter.com/QFz78yi28o</a></p>&mdash; Eric L. Barnes (@ericlbarnes) <a href="https://twitter.com/ericlbarnes/status/1138528829692174337?ref_src=twsrc%5Etfw">June 11, 2019</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>
{% endraw %}

When creating an array, reference types are stored as an array of references, where as value types are stored directly in the array.  In order to access the data of a reference type, you first have to go to the memory location of the reference, then do what you need with the data there.  Value types on the other hand point directly to the memory location where the data is stored.  The best was I can describe this is with a picture.

![Array of classes versus array of structs](/img/array-of-classes.png)

In the array of classes, the array itself only contains references to the memory locations, and if you want to access them, you need to go look up that memory location before you can use the data in that class.  If we instead switch that class to a struct, then we end up in the situation of the second part of the image above.  In this case, we avoid the extra lookup, and gain some performance.

From a code perspective, this change seems trivial:

{% codeblock lang:csharp %}
public class DecisionTreeNode { /* ... */ }
// becomes
public struct DecisionTreeNode { /* ... */ }
{% endcodeblock %}

Don't go switching all your classes to structs just yet however.  There are a lot of concerns to consider when deciding if an entity should be a class or a struct and there are plenty of other places on the internet where you can find good discussion about this topic.  In this instance, the respresentation of my entity is not intended to be exposed beyond my library, which gives us a lot of freedom around it's design and so we optimize for performance.

Under the assumption we are only going to expose the `RandomForest` class on our API and not the `DecisionTree`, we can use this same technique for the `DecisionTree` class to struct as well.  We won't see nearly as large of a speedup however because we're accessing the trees many fewer times than the nodes and we still have to pay the price to lookup the nodes array reference.

> [6/28/2019 12:48:32 AM] Time taken for 5000 evaluations: 184.1993 ms

Holy cow those extra lookups appear to have been quite impactful.  We're now at a 41.82% total reduction in time.  If you were still shaking your stick after the last one, I can see your argument, but after this one, now you just look crazy.  Put the stick down.

Some things to note about this change...

Part of our performance win comes from the way properties are handled in classes versus structs.  Structs cannot be inherited from, so when it calls the function to get the value of the property, it can be sure which function exactly needs to be called.  Since classes can be inherited from and the functions can be overridden, the actual function that needs to get called has to be looked up based on the type of the class pushed to the reference.  This is how you can have two different implementations of the same base class, and still the correct function gets called even when you are holding a reference to the base class and not actual implementation.

In the case that your properties are simply wrappers to a field, you can explicitly eliminate the getter calls by just making them fields instead properties.  If you are compiling with optimizations on (default for release builds) then the JIT appears to be smart enough to inline the property calls for you anyway and you don't need to worry about it.

And lastly, since structs cannot be null, there is no point in the function call checking if the instance is null or not which saves some additional work.

### Shrinking the representation

There are tons of trees, 1,000 to be exact, and each of them can have up to 64 nodes each, and each tree node has 3 ints and 1 double.  In practice there are 50,488 total tree nodes in the full model and each one is using 20 bytes each, for a total size of about 2.5 MB.  During any given request we have to run 5,000 jobs through these 2.5 MB{% raw %}<sup>1</sup>{% endraw %} and what you end up with is a lot of churn.  Since we cannot really change the feature representation without changing the model, we should look to reduce the size of the trees themselves in order to reduce the memory churn.

{% raw %}<small><sup>1</sup> This is a bit of an exaggeration because we're only going to touch about 5,000 nodes on average for a single evaluation based on the path taken through the trees.<small>{% endraw %}

This point is a bit of a crossroads in the design process for this solution.  We _can_ shrink the representation, but each time we do we're effectively making this implementation less and less a general tool and moving more in the direction of trying to solve the specific model given to us.  In any case, we have a long way to go right now in our performance quest, so let's go full specific model and once we get to our goal, _if_ we get to our goal, we'll back it out and try to make it more general purpose.  The decision is made.

Considering we only care about this specific model, we know there are only 840 features, and you only need a short to store all possible feature indices.  We also know that each tree contains no more than 64 nodes, so our branches only need to be the size of individual bytes.  And lastly, as long as verification test passes, we're willing to accept any size reduction in the value representation as well, so let's try a float instead of a double.

{% codeblock lang:csharp %}
public struct DecisionTreeNode
{
    public short FeatureIndex;
    public float Value;
    public byte TrueBranch;
    public byte FalseBranch;
}
{% endcodeblock %}

> [6/28/2019 1:29:09 AM] Time taken for 5000 evaluations: 157.3987 ms
> 
> [6/28/2019 1:29:10 AM] Correctness verified.

Now we're down to 8 total bytes per node.  Our full model size just went from about 2.5 MB down to 404 KB.  And it looks like we're good to go on the correctness so we'll take it.  We shaved off another 27 ms bringing us to a total of 51.29% time reduction.  

But hold up, there's one more issue we really need to address that was made prominent by this last change.  When you want to access a data type that is 4 bytes wide (the `Value` in this case), it's more efficient to align it in memory to an address that is divisible by 4.  There is a lot of nuance around this which is beyond the scope of the discussion here, but this problem pops up really bad when you are basically guaranteeing this alignment will be off, which is what we did by changing the `FeatureIndex` from an `int` (4 bytes) to a `short` (2 bytes) and forcing the full size of the `DecisionTreeNode` to exactly 8 bytes.

When you ask for memory, most memory allocators are aware that aligned memory is more performant and will give you memory starting with an aligned address.  In this case our `FeatureIndex` is accessible at the aligned address, however the `Value` is not and it's 4 bytes in length.  Since our full data structure is exactly 8 bytes, and it's an array of structs, we're pretty much in a bad spot.  This alignment issue means it can be a fair bit more expensive to load the non aligned value.  Luckily, the fix is really simple, you just swap the order of the fields in your class definition.

{% codeblock lang:csharp %}
public struct DecisionTreeNode
{
    public float Value;
    public short FeatureIndex;
    public byte TrueBranch;
    public byte FalseBranch;
}
{% endcodeblock %}

> [6/28/2019 1:52:04 AM] Time taken for 5000 evaluations: 133.6665 ms

And there we go.  Better alignment for 4 byte data types, better performance.  58.72% total improvement so far.

### Fast access to the root of each tree

This last one is a bit of a hack.  We just spent a bunch of time squeezing out chunks of performance from the tree nodes which get used a ton, let's take a step back and see if we can speed up dealing with whole trees.  

Since the trees contain references to variable sized arrays, we wont see much of a win by turning them into structs because we're still going to pay the additional lookup price.  What we can do however is add a field to the trees that is a copy of the first node so that we effectively have the first node right next to the reference in our array of trees.  Essentially, at the same time that we get the reference to all nodes, we also get the full root node of the tree available, and we can start processing the first iteration of the loop while some magic happens in the background to prefetch the first page of memory containing the nodes.

It should be noted that this only works because our tree node is a struct, and therefore lives right in line on the same memory that has the reference to the first array node.  If the `DecisionTreeNode` was a class, this would not work because it would store a reference to the exact same memory location at the front of the array of all nodes, and we would still eat the latency of that extra lookup.  This also would not work if our work loop in the `Evaluate` method was a tight loop with very few instructions.  Because we're doing a lot of work in here, we get the option to hide the latency of the memory lookup by pulling the useful information at the same time we load the reference to the tree.

> [6/28/2019 2:00:50 AM] Time taken for 5000 evaluations: 116.4636 ms

And another 17 ms bites the dust bringing us to a grand total of 63.21% reduction in time.  That's not too shabby for literally just changing the implementation details without altering the overall approach.  We're now within a factor of about 4 to 5 of where we need to be, down from a factor of 12 to 13.  A good days work.

Of all the patterns we've exploited so far, this last one I expect you will end up using the least; again, it's basically a hack.  I decided to include this here because it really highlights the impact of optimizing for memory access latency.  By getting some work in while we wait for the array memory to load, we were able to shave off 13% of the time on something that's already getting pretty fast.  At this point we're firmly in the land of micro-optimizations, but at this scale, things like this can have a dramatic impact.

Let's take a look at where we're at:

{% codeblock lang:csharp %}
public struct DecisionTree
{
    public struct DecisionTreeNode
    {
        public float Value;
        public short FeatureIndex;
        public byte TrueBranch;
        public byte FalseBranch;
    }

    private readonly DecisionTreeNode[] nodes; // set in the constructor
    private readonly DecisionTreeNode root; // set in the constructor

    public double Evaluate(double[] features)
    {
        var node = root;
        while(node.FeatureIndex != -1) // LeafIndex = -1
        {
            int nodeIndex = features[node.FeatureIndex] < node.Value ? node.TrueBranch : node.FalseBranch;
            node = nodes[nodeIndex];
        }
        return node.Value;
    }
}

public sealed class RandomForest
{
    private readonly DecisionTree[] trees; // set in the constructor

    public double EvaluateProbability(double[] instance)
    {
        var sum = 0d;
        for(int i = 0; i < trees.Length; i++)
        {
            sum += trees[i].Evaluate(instance);
        }
        return 1f / (1f + Math.Exp(-sum)); // conversion to probability
    }
}
{% endcodeblock %}

Looking at the differences, even I'm amazed at the performance difference this code has compared to where we started at the top.  We did not have to modify our approach at all, we just had to take advantage of some knowledge about what was happening under the hood.

Hopefully you'll start to recognize these common patterns in your own work and you'll remember this blog post.  Showing up to your cadence meeting and presenting a 63% performance improvement on something that runs hundreds of times per second is always a fun experience.  

### What's next?

When you run out of common patterns to exploit, it's time to start pulling out the tools to help you identify the slow parts.  Next time we'll run through some tools which helped us identify the remaining bottlenecks in our code.

If you are interested in following along and trying this stuff out for yourself, please clone [the repo](https://github.com/culture-of-development/random-forest-perf-blog) and run the naive tests as shown in the readme.