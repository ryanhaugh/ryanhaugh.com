---
layout: post
title: "Neat VS10 Feature: Pinning A Debugger Watch"
date: 2009-11-06 -0800
comments: true
disqus_identifier: 18657
categories: []
---
I was stepping through some code in a debugger today and noticed a neat
little feature of Visual Studio 2010 that I hadn’t noticed before.

When debugging, you can easily examine the value of a variably by
highlighting it with your mouse. Nothing new there. But then I noticed a
little pin next to it, which I’ve never seen before.

![debugger-value](http://haacked.com/images/haacked_com/WindowsLiveWriter/NeatVS10FeaturePinningADebuggerWatch_EB08/debugger-value_3.png "debugger-value")

So what do you see when you see a pin? You click on it!

![pinned-quick-watch](http://haacked.com/images/haacked_com/WindowsLiveWriter/NeatVS10FeaturePinningADebuggerWatch_EB08/pinned-quick-watch_3.png "pinned-quick-watch")

As you might expect, that pins the quick watch in place. So now I hit
the play button, continue running my app in the debugger, and the next
time I hit that breakpoint:

![pinne-watch-changed](http://haacked.com/images/haacked_com/WindowsLiveWriter/NeatVS10FeaturePinningADebuggerWatch_EB08/pinne-watch-changed_3.png "pinne-watch-changed")

I can clearly see the value changed since the last time. I think this
may come in useful when walking through code as a way of seeing the
value of important variables right next to where they are declared. I
thought that was pretty neat.

