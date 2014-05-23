---
layout: post
title: "An Even EASIER Way To Obtain the Subtext Source Code"
date: 2005-06-21 -0800
comments: true
disqus_identifier: 7116
categories: []
---
In a [recent post](http://haacked.com/archive/2005/06/18/5155.aspx) I
outlined step by step how to obtain the source code for
[Subtext](https://sourceforge.net/projects/subtext/) as a non-developer.
Well I was a bit sloppy and made a couple of mistakes in the post (now
corrected).

However, I feel a bit bad about it. It’s not like me to be so sloppy and
give incorrect information. I decided I must pay penance! These
instructions apply to those who want to look at the source code on a
development machine.

### The Easier Way!

I created a simple [batch file](/code/GetSubtext.zip) you can use to
execute the CVS command for you. So my updated steps are:

-   As before, make sure you have
    [TortoiseCVS](http://www.tortoisecvs.org/) installed.
-   Create a database in SQL Server named “SubtextData” and make sure
    your database server has an ASPNET login.
-   Create an ASPNET user using the ASPNET login for the SubtextData
    database.
-   [Download and unzip](/code/GetSubtext.zip) the batch file to your
    projects folder (for me, I would place this file in my
    “c:\\Projects” folder.
-   Now double click the batch file.

If you install TortoiseCVS in a location other than your program files,
you will need to modify the batch file to point to the correct location.

When you run the batch file, it downloads all the source code from CVS
and then runs a downloaded script named “CreateSubtextVdir.vbs” that
will set up a virtual directory in IIS pointing to the correct location.
All you have to do is compile and browse to
http://localhost/Subtext.Web/.

Again, I hope this makes it easier to grab the latest bits and play
around with it.

[Listening to: rez/Cowgirl - Underworld - Everything Everything: Live
[IMPORT] [LIVE] (11:47)]

