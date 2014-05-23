---
layout: post
title: "Change The Display Of A Type In The Debugger"
date: 2006-10-04 -0800
comments: true
disqus_identifier: 17634
categories: [code]
---
[Keyvan Nayyeri](http://keyvan.io/ "Keyvan Nayyeri's Blog") has [a great
tip](http://keyvan.io/simpler-debugger-with-debuggertypeproxy "Simpler Debugger With DebuggerTypeProxy")
for how to control the display of a type in the various debugger windows
using a `DebuggerTypeProxy` attribute.  His post includes screenshots
with this in use.

This is an attribute you can apply to a class, assembly, or struct to
specify another class to examine within a debugger window. 

You still get access to the raw view of the original type, just in case
some other developer plays a practical joke on you by specifying a
`DebuggerTypeProxy` that displays the value of every field as being 42.

