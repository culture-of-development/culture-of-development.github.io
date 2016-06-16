---
title: "Open source and the ability to modify code"
author: Nick Larsen
date: 2013-02-28 09:29
categories: blog
---
If you google for the [advantages of open source software][1], you will get results that enumerate a few ideas like this:

1. It's cheaper!
2. It's more secure!
3. No vendor lock!
4. Better quality!
5. Auditability!
6. Customizability!
7. ???
8. ...

And if you read what they write under customizability, what they mean is that you can modify the code for your own system.   Modifying open source software for your own system is *highly inadvisable*.  As soon as you make a change, you have forked the project and forked projects are often difficult or impossible to keep up to date with changes in the master branch.  

### So open source software is just for using, not for editing?

No, of course not.  If you need to make tweaks or other changes to the code of an open source project, you should be **contributing** your code to those projects, and integrating the new features to your system once your code is merged.

On the [Careers 2.0](http://careers.stackoverflow.com/) project I write code for, all of our open source dependencies are maintained through [nuget](http://nuget.org/), even the ones Stack Exchange [employees](https://github.com/ServiceStack) [have](https://github.com/emmettnicholas/StacMan) [contributor](https://github.com/NickCraver/StackExchange.Exceptional) [access](https://github.com/ServiceStack/Bundler) [to](https://github.com/StackExchange/MiniProfiler).  If you want to make a change, submit a pull request, have it accepted, publish the updated nuget package and update the nuget reference in the project.  It sounds like a long path, but it keeps your system up to date and makes all of your contributions public.

### But what if there is a bug or a critical vulnerability?

There are exactly 1 situations I can think of when it is acceptable to modify an open source project, compile it and deploy it within your system:

1. You need an immediate fix for a vulnerability that must go live until the fix is merged into the master.

When the project has a vulnerability, I'm not going to argue taking anything but the shortest line approach to fixing it.  If it's just a bug, that's just code for no better time to submit a pull request!


[1]: https://www.google.com/webhp?sourceid=chrome-instant&ion=1&ie=UTF-8#hl=en&sclient=psy-ab&q=advantages%20of%20open%20source%20software&oq=&gs_l=&pbx=1&fp=abb3d512a9f598c1&ion=1&bav=on.2,or.r_gc.r_pw.r_cp.r_qf.&bvm=bv.43148975,d.eWU&biw=1264&bih=1487