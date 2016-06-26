---
title: Build Your First Thing With WebAssembly
categories: blog
date: 2016-06-24 18:48:56
author: Nick Larsen
---

When I first heard of [WebAssembly](https://webassembly.github.io/) it sure sounded cool and I was super excited to start trying it out.  As soon as I tried to get started however, it got a lot more frustrating.  My goal here is to save you some of that frustration.

![beware of cliff](/img/cliff.jpg)

### Reader beware

This post was written in June 24th, 2016.  WebAssembly is a very new and unstable technology and everything in this post might be wrong as the process continues to standardize it for all browsers.

With that out of the way....

### What is WebAssembly?

Well the official website describes it as:

>> WebAssembly or wasm is a new portable, size- and load-time-efficient format suitable for compilation to the web.

Uhh... wat?  Format what?  Text?  Binary?  Frankly this is just a bad description.  So instead, get out your buzzword bingo cards and I'll wrap up all my experience with wasm and come up with my own description:

>> WebAssembly or wasm is a bytecode specification for writing performant, browser agnostic web components.

So we're still not quite to the point of sounding totally awesome with a quip, but here comes the rest of the elevator pitch.  WebAssembly realizes performance gains by using statically typed variables which are much more efficient to reference than dynamically typed varaibles at runtime.  WebAssembly is being developed by a [W3C Community Group](https://www.w3.org/community/webassembly/) to eventually be supported by all spec compliant browsers.  And the killer feature, _eventually_ you'll be able to write these web components in _any_ language.

Sounds a lot cooler now, doesn't it?

### Let's get started

When it's time to learn a new thing, I usually look for the smallest possible example to see it working.  Unfortunately for us all, that's not a reality with WebAssembly.  In it's current state, wasm is basically a specification for bytecode.  Imagine if back in 1996, some engineers from Sun Microsystems introduced the JVM... but no Java.  I imagine that conversation going a little like: "Hey y'all, come check out this bytecode executing virtual machine we made!" "Cool! How do we write code for it?"

![HelloWorld.class](/img/jvm-bytecode.png)
*HelloWorld in bytecode*

"Uhh.. cool man.  I'll check it out sometime." "Awesome, let us know what you think and drop us a line on our github page if you run into any trouble." "You got it, we're gonna check out some of the other projects over here now."

And even that's a bad example because the JVM is based on the Java language, but hopefully you get the point.  If your byte code doesn't show up on the scene with tools that compile to it, you're gonna have a hard time getting off the ground.  So how do we actually get started then?

### What came before WebAssembly

Most technology is the result of innovation, especially when a reasonable attempt is being made to make it a formal specification.  Wasm is no different, it is effectively a continuation of the work done on [asm.js](http://asmjs.org/), a specification for writing javascript components in a way that can be compiled with static typing.  Wasm extends those ideas by writing a specification for bytecode that can be targeted by a compiler of any language, sent over the wire as a binary file instead of text encoding and developed over time by representatives from many of the major browsers instead of just Mozilla.

asm.js is just a specification for writing javascript using a minimal subset of the language features.  You can write simple asm.js by hand, and if you just want to get your hands dirty, it's definitely the way to start.  (it's best to put this in it's own file for later on, and the convention is to use `your-module-name.asm.js`)

    function MyMathModule(global) {
        "use asm";
        var exp = global.Math.exp;
        function doubleExp(value) {
            value = +value;
            return +(+exp(+value) * 2.0);
        }
        return { doubleExp: doubleExp };
    }

This isn't a particularly useful function, but it is to spec.  If it looks silly to you, you're not alone, but almost every character there is necessary.  All of those unary `+` operators act as type annotations for the compiler so it knows that they are doubles and doesn't have to figure out what they are at runtime.  It's super finicky but if you mess something up, the console in firefox will give you a reasonable error message to work off of.  

If you want to use it in a browser, something like this works:

    var myMath = new MyMathModule(window);
    for(var i = 0; i < 5; i++) {
        console.log(myMath.doubleExp(i));
    }

And if you got all that correct, the output should look something like this:

![asm.js success](/img/asmjs-success.png)

### Get to the WebAssembly already

Now that we have a working piece of asm.js, we can use the tools provided by the [WebAssembly github page](https://github.com/WebAssembly/binaryen) to compile it to wasm.  Now you have to clone the repo and build the tools yourself.  This is the worst part.  These tools are under constant development and it's a common occurrence that they break from time to time, especially if you're on windows.

You're definitely going to need make and cmake with commandline tools installed on your system regardless of whether you're on windows or mac.  If you're on windows, you'll also need a version of visual studio 2015 installed.  If you're on a mac, follow [these instructions](https://github.com/WebAssembly/binaryen#building), and if you're on windows, follow [these instructions](https://github.com/brakmic/brakmic/blob/master/webassembly/COMPILING_WIN32.md).

![building binaryen](/img/binaryen-windows-build.png)
*Building binaryen on windows*

Distributing working binaries would be a huge step in the right direction for the WebAssembly team.

If you managed to get through this successfully, a bin folder was created in the binaryen directory with some tools we can use to convert our asm.js to wasm.  The first tool is `asm2wasm.exe`.  This tool compiles the asm.js code to `.s` format code, which is a textual representation of the abstract syntax tree (AST) for the wasm output.  Once you run the utility, you end up with something that looks like this:

    (module
      (memory 256 256)
      (export "memory" memory)
      (type $FUNCSIG$dd (func (param f64) (result f64)))
      (import $exp "global.Math" "exp" (param f64) (result f64))
      (export "doubleExp" $doubleExp)
      (func $doubleExp (param $0 f64) (result f64)
        (f64.mul
          (call_import $exp
            (get_local $0)
          )
          (f64.const 2)
        )
      )
    )

In the future we can disect this line by line, but for now I just want to show it to you and mention that since wasm is a binary format, right clicking and viewing source like you can do for any javascript today just doesn't work; it would look a lot like that byte code up top.  The current plan is that when you view source on a wasm module, it would disassemble to this format to make it human readable.

The next thing we need to do is convert this `.s` format code to a wasm binary, and to do that we use `wasm-as.exe`, for wasm assembler.  Once you run this file, you'll end up with the actual wasm bytecode we need for the browser.

![building wasm from asm.js](/img/binaryen-transform.png)
*transforming asm.js to a wasm binary*

![wasm bytecode](/img/wasm-bytecode.png)
*wasm bytecode*

Next, grab yourself the latest copy of [Firefox](https://www.mozilla.org/en-US/firefox/new/) or [Chrome Canary](https://www.google.com/chrome/browser/canary.html) and enable WebAssembly.  

For Firefox, you'll need to go to `about:config` in the url bar, and then tell it you'll be careful.  After that, type `wasm` in the search bar and double click `javascript.options.wasm` until the value is set to `true` and then completely restart the browser.  

For Chrome Canary, you'll need to go to `chrome://flags` and scroll down until you find `Experimental WebAssembly`, click the enable link and completely restart the browser.

The last step is get the module running in the browser.  This was another pain point when I first started because it's totally hidden.  I couldn't anything in the spec related to the javascript API for using wasm modules.  I ended up just opening up a console in Chrome Canary and typing `WebAsse` and nothing popped up.  Next I tried typing `Was` and then it popped up!  Looking at that object in the inspector was about the poorest documentation ever, but then it occurred to me to use some other tools (emscripten) that compiles to wasm.  How I did that is the topic of another blog post, but after doing that I was able to create a working example.

Some time later I was clicking around and eventually ended up in the design repo for WebAssembly.  There I saw a file named [JS.md](https://github.com/WebAssembly/design/blob/master/JS.md) so I clicked on it and sure as shit, there was an actual javascript API with documentation.  Pay close attention to the italic note at the top.  The best part of this page however was the snippet at the very bottom which shows you how to load up a module very minimally.  All I had to do was switch out the relevant parts and try it out.

    fetch("my-math-module.wasm")
        .then(function(response) {
            return response.arrayBuffer();
        })
        .then(function(buffer) {
            var dependencies = {
                "global": {},
                "env": {}
            };
            dependencies["global.Math"] = window.Math;
            var moduleBufferView = new Uint8Array(buffer);
            var myMathModule = Wasm.instantiateModule(moduleBufferView, dependencies);
            console.log(myMathModule.exports.doubleExp);
            for(var i = 0; i < 5; i++) {
                console.log(myMathModule.exports.doubleExp(i));
            }
        });

Toss that in an html file, [fire up a server in your local folder](https://www.npmjs.com/package/local-web-server) and load it up.  Here's what it looks like in both browsers:

![wasm in a browser](/img/wasm-in-browser.png)
*wasm running in a browser (or trying to at least)*

Time to go file a bug report I guess.  Remember, it's all experimental technology and very unstable, so try not to get overly frustrated when things like this happen.

![keep calm and file bug reports](/img/keep-calm-and-file-bug-reports.png) 

### Congratulations!

You've created your first WebAssembly component.  What's next?  We're really just cracking the surface here.  Writing asm.js by hand was pretty cruical to this example, but doing anything non trivial in it takes a long time and lot of patience.  Using emscripten to compile non trivial apps to asm.js is much easier.  On that note, I highly suggest reading up on the asm.js specification, especially the memory model, because a lot of the concepts from there carry over to WebAssembly.  Another quirk is that right now you cannot directly pass arrays as function arguments.  There is some agreement that that should change, but nothing has made it in the spec yet.  Go brush up on your pointer logic.

Another note, when you start doing non trivial things in wasm, you might find that it actually performs slower in WebAssembly than plain old javascript.  Just remember that modern javascript engines are highly optimized for compiling javascript and it will take time for wasm compilation to catch up.  WebAssembly is not ready for production usage.

If you have any questions regarding wasm or the tooling described here, please ask on [Stack Overflow](http://stackoverflow.com) and tag appropriately.

