---
title: 'How we sped up random forest processing, lots of common mistakes'
author: Nick Larsen
categories: blog
date: 2019-06-21 22:43:18
---


In [the previous article](/blog/how-we-sped-up-random-forest-processing-getting-the-lay-of-the-land/) we outlined our problem, the major constraints we must adhere to and wrote a straightforward implementation of a random forest evaluator.  We proved the correctness of the implementation and then we set up a naive benchmark to test how much time it was going to take to run 5,000 samples through 1,000 trees, each of a maximum depth of 5 decision nodes.  This currently runs in about 320 ms and we need it to run in about 25 ms.

As a quick reminder, here is a condensed version of the code from the previous article, ignoring most of the boilerplate:

{% codeblock lang:csharp %}
public sealed class DecisionTree
{
    public struct DecisionTreeNode
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

### The linq tax

Linq is an amazing technology that allows you to write extremely expressive code using a declarative syntax.  Want to sum up the results of calling a function on each and every element of an array?  One liner.  And reads almost like the English sentence I just used to describe it.  It's a wonderful tool.

But it has some drawbacks.  The work-performing argument to nearly every linq operation is a function.  If you're not careful about the functions you use as arguments, you might be leaving a fair amount of performance lying on the floor.  

Usually, as in this example above on line 31, the function argument is passed as a lambda function.  When a lambda function is executed, it runs in the context in which it was declared, meaning it has access to the variables in that scope.  This is how we are able to pass in the `instance` argument which is a local variable to the `EvaluateProbability` function without declaring the `instance` variable in the lambda itself.

Creating the context for a lambda function is a relatively expensive task.  You won't notice it if you only ever generate  few contexts for a particular lambda, but if you are doing it over and over, the taxes start to add up and in our case we're doing this about 5,000 times per request.  

There are a few ways to solve this problem, but the two most straightforward methods are to either 1) don't use a lambda function where it's not necessary, or 2) don't use linq. In this case we do need the context in order to get the `instance` variable, so fix #1 isn't really an option and we should just get rid of our use of linq.

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

> TODO: [6/22/19 3:26:14 AM] Time taken for 5000 evaluations: 159.6062 ms

That's a 23.81% reduction in time; that's nothing to shake a stick at.  Let's be really clear when this kind of optimization matters.  When a user visits the site, we need to perform this calculation for roughly 5,000 jobs, and at peak hours we're going to be handling roughly 800 requests per second.  This code gets called _a lot_.

If this were only happening once per request, we wouldn't worry about this kind of optimization because 800 times just isn't that much.  But when you have code that's being called 4,000,000 times per second, for hours on end, the linq tax adds up very fast.

### Arrays of classes

In .NET, there are classes, which are reference types, and there are structs, which are value types.  Classes are usually larger data structures in terms of maximum possible bytes used and they can be null where as structs are usually smaller and they can not be null (with the caveat of `Nullable<T>`).  When passing a value type to a function or setting a new variable to a value type, a full copy is made, where as with reference types, only the reference is copied.

{% raw %}
<blockquote class="twitter-tweet" data-lang="en"><p lang="en" dir="ltr">This gif never gets old. <a href="https://t.co/QFz78yi28o">pic.twitter.com/QFz78yi28o</a></p>&mdash; Eric L. Barnes (@ericlbarnes) <a href="https://twitter.com/ericlbarnes/status/1138528829692174337?ref_src=twsrc%5Etfw">June 11, 2019</a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>
{% endraw %}

When creating an array, reference types are stored as an array of references, where as value types are stored directly in the array.  In order to access the data of a reference type, you first have to go to the memory location of the reference, then do what you need with the data there.  Value types on the other hand point directly to the memory location where the data is stored.  The best was I can describe this is with a picture.

TODO: picture of array of reference, picture of array of structs

In the array of classes, the array itself only contains references to the memory locations, and if you want to access them, you need to go look up that memory location before you can use the data in that class.  If we instead switch that class to a struct, then we end up in the situation of the second part of the image above.  In this case, we avoid the extra lookup, and gain some performance.

You can actually see this extra work if you look at the intermediate language (IL) that C# compiles down to when you use the compiler.