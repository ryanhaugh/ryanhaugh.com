---
layout: post
title: "Adding a Windows Service Description"
date: 2004-11-02 -0800
comments: true
disqus_identifier: 1550
categories: []
---
When building an installer for a Windows Service in VS.NET 2003,
conspicuously missing is the ability to specify a description for the
service that is displayed in the Services applet.

I've written a base installer class that inherits from
System.Configuration.Install.Installer for this purpose, but I'll just
present to you the source listings for the methods to add and remove a
service description.

Check [it out here](/articles/1549.aspx). I hope you find it useful.

