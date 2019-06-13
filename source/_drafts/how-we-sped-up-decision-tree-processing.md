---
title: How we sped up decision tree processing
author: Nick Larsen
categories: blog
date: 2019-06-06 12:46:41
---

## introduction
- the goal for the whole project
  - describe the project, show where on the page it is being used
  - describe what we currently have and why it's not keeping up with the times
  - we need to test it
- we tried to most simple thing first
  - keep the model in the language it was developed in
  - that was attrocious
  - yea that's not gonna work
- the requirements for the whole project
  - our goal is not to make a general purpose tool, but make this specific case as fast as possible
  - 5000 jobs
  - 99th % at like 85 ms, realistically it's more like 50 ms
  - must be single threaded, it's running on the same boxes as everything else that is publicly web facing
  - soft requirement on how much memory it can use, just needs to be as little as possible

## getting it up and running
- switch to c# land
  - parsing the provided model files
  - the most naive possible solution
    - show all the necessary code in a snippet
  - verifying correctness

## performance tuning

### the trivial stuff
- shrinking the representation
- using structs instead of classes for nodes
- putting class fields in the best order
- get rid of linq
- getting fast access to the first node

### finding out the real (i.e. actual) slow parts
- get some ideas, like where we think it's slow
- verify those ideas
- do some profiling (simple timers)
- some crazier profiling (visual studio profiler, dotTrace)
- some insane profiling (vtune)
- problems with all these methods

## the overall problem
- cache pressure
- how did we decide it was cache pressure?

## general approaches to fixing it
- reduce the memory size (hey we already did that once!)
- reduce the working set size
- access data in predictable patterns

## changes to the decision trees
- remember we're just trying to solve our specific use case, not general purpose trees

### approach: reorder the features
- idea is that the features jump all over the place, if we put commonly accessed features close to each other, it should greatly reduce the number of cache pages that we need
- took the greedy approach
- describe the evaluation function

- To parallel comparing sorting/searching algorithms. Introduce it, explain it, show it, why would it work/not work for this scenario : @theHugoDahl

### approach: evaluate all samples for 1 tree at a time
- take a step back and look at the whole problem, not just optimize the thing the profiler said was slow, but focus on the whole problem
- there were 2 places we loaded from memory, 1 was features, other was jobs, so this is the next likely approach
- also the trees are quite heavy and churning through all that data is expensive, so we should reduce the churn
- guess what it's wayyy faster!

### approach: compiled trees
- there is always an extra branch at the end, can we eliminate the extra branch?
- let's code gen a function for each tree to reduce it as much as possible
- this was insanely fast
- guess what it's also got a huge bug
  - the bug that pretty clearly identified it as cache pressure though which really narrowed down the future ideas

### approach: better memory alignment for the trees
- the trees were stored in objects that were spread out in memory
- what if we put them all close together?
- array of structs to struct of arrays
- can we also get rid of the indexes for the branches because that would eliminate some lookup and shrink the size even more
- this was an actual win

### approach: is precomputing possible?
- this is something we had been thinking about all along
- but we tried repeatedly to precompute the jobs, only because these are branches, you now have to create a set of trees for each job which is an asinine amount of memory usage which is beyond our allowance
- instead let's take _another_ step back and look at the whole test, compared to what's going to happen in production
- there's only 1 user, so we can precompute the user then run it for all jobs
- this was another huge win

## getting ready for production
- making the decision which algorithm to choose
- eliminating allocations
- reducing memory churn as much as possible
- making a test that more closely mimics production use
- cleaning up the code for readability and documentation