---
layout: post
title: "Which Logging framework do you use and why?"
date: 2004-02-11 -0800
comments: true
disqus_identifier: 190
categories: []
---
![Logs](/images/logging.jpg) Scott Hanselman asks the question [Should
one go to Production with a Debug
build?](http://www.hanselman.com/blog/PermaLink.aspx?guid=495312ae-08a1-4712-b654-e392bf34bfd2 "Going to production")
Although I find that an intriguing question, I’m more intrigued by the
fact that his company exclusively uses Log4Net exclusively.

In my current project, I am using the built in Tracing framework that is
part of the System.Diagnostics namespace. It supports multiple logging
levels and you can add custom Trace listeners. So my question is, what
does a framework like Log4Net offer that the Tracing framework doesn’t?

