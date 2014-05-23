---
layout: post
title: "How Subtext Uses CruiseControl.NET"
date: 2006-05-18 -0800
comments: true
disqus_identifier: 12845
categories: []
---
Not too long ago [I mentioned
that](http://haacked.com/archive/2006/05/03/SubtextCruisingInCruiseControl.NET.aspx "Subtext CruiseControl.NET")
the Subtext team is using
[CruiseControl.NET](http://ccnet.thoughtworks.com/ "CruiseControl.NET Home")
for continuous integration. Well [Simone
Chiaretta](http://blogs.ugidotnet.org/piyo/ "FoxyBlog (in italian)"),
the developer who set this up, [wrote up an article describing
Continuous
Integration](http://www.subtextproject.com/Home/Docs/Developer/ContinuousIntegration/tabid/145/Default.aspx "Continuous Integration and Subtext")
and the various utilities that Subtext uses in its CI process.

![](http://haacked.com/images/CCNetTrayScreenshot.gif)

As you can see in the screenshot, the last build succeeded. Check out
this small snippet from our [NCover
report](http://haacked.dyndns.org/ccnet/server/local/project/SubText/build/log20060518054054Lbuild.1.4.1.69.xml/NCoverBuildReport.aspx "NCover report")

![](http://haacked.com/images/NCoverReport.gif)

As you can see, we have a bit of work to do. But remember, [code
coverage isn’t
everything](http://haacked.com/archive/2004/11/03/CodeCoverageIsNotEnough.aspx "Code Coverage Isn't Enough").

