---
layout: post
title: "Supporting STA Threads in Web API"
date: 2014-05-24 -0700
comments: true
categories: [web-api]
---

I was reading [this blog post](http://weblog.west-wind.com/posts/2012/Sep/18/Creating-STA-COM-compatible-ASPNET-Applications) about how to support STA threads in ASP.NET and the post has a solution for all ASP.NET technologies except for Web API.  He added a comment saying he couldn't find a way to do it and opened it up for someone else to solve.

Challenge accepted!

But before I describe how to do this, I should explain *why* we'd want to do this.