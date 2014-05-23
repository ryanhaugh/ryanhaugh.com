---
layout: post
title: "Share In My Stupidity"
date: 2006-09-19 -0800
comments: true
disqus_identifier: 16848
categories: []
---
[![Dumb and
Dumber](http://haacked.com/images/haacked_com/WindowsLiveWriter/ShareInMyStupidity_9F1F/dumb-and-dumber-001-1_thumb.jpg)](http://haacked.com/images/haacked_com/WindowsLiveWriter/ShareInMyStupidity_9F1F/dumb-and-dumber-001-1%5B2%5D.jpg)
In general I like to regale my readers with stories of my brave
accomplishments, ideally embellished to make me look like a hero. 

But last night I was dealing with a problem that when I realized the
solution, I knew I deserved a [big red
WTF](http://www.codinghorror.com/blog/archives/000679.html) on my
forehead.

I was playing around with an Atlas UpdatePanel in a form on some
existing code.  No matter what I tried, the site would perform what
appeared to be a full post back.  I started cursing Atlas and it’s
gee-whiz-bang-newfangled-broken UpdatePanel.

This morning, before work I thought I would take a quick look at the
underlying code behind (not sure why I didn’t do this last night). 
Right there in the submit button event handler was the following line of
code (actually slightly modified for brevity).

```csharp
Response.Redirect("/Here.aspx?msg=Thanks");
```

I had totally forgotten that there was a redirect in response to that
button event!  So the UpdatePanel was working just fine.  The apparent
post back was actually a redirect.

See, that's the problem with software. It does **exactly** what you tell
it to do. Even when you mean otherwise.

tags: [Atlas](http://technorati.com/tag/Atlas),
[WTF](http://technorati.com/tag/WTF)

