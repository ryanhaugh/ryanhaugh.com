---
layout: post
title: "Using GitHub for Windows with non-GitHub repositories"
date: 2012-05-30 -0800
comments: true
disqus_identifier: 18859
categories: [github,git]
---
In my [last blog
post](http://haacked.com/archive/2012/05/21/introducing-github-for-windows.aspx "Introducing GitHub for Windows"),
I mentioned that GitHub for Windows (GHfW) works with non-GitHub
repositories, but I didn’t go into details on how to do that. GHfW is
optimized for [GitHub.com](http://github.com/ "GitHub") of course, but
using it with non-GitHub repositories is quite easy.

All you need to do is drag and drop the HTTPS clone URL into the
dashboard of the application.

For example, suppose you want to work on a project hosted on
[CodePlex.com](http://codeplex.com/ "CodePlex.com"). In my case, I’ll
choose [NuGet](http://nuget.codeplex.com/ "NuGet on CodePlex"). The
first thing you need to find is the Clone URL. In CodePlex, click on the
*Source Code* tab and then click on the sidebar *Git* link to get the
remote URL. If there is no Git link, then you are out of luck.

[![nuget-codeplex-git-url](http://haacked.com/images/haacked_com/Windows-Live-Writer/0053a500d158_1223E/nuget-codeplex-git-url_thumb.png "nuget-codeplex-git-url")](http://haacked.com/images/haacked_com/Windows-Live-Writer/0053a500d158_1223E/nuget-codeplex-git-url_2.png)

Next, select the text of the clone url, then click on it and drag it
into the GitHub for Windows dashboard. Pretty easy!

[![ghfw-drag-drop-repo](http://haacked.com/images/haacked_com/Windows-Live-Writer/0053a500d158_1223E/ghfw-drag-drop-repo_thumb.png "ghfw-drag-drop-repo")](http://haacked.com/images/haacked_com/Windows-Live-Writer/0053a500d158_1223E/ghfw-drag-drop-repo_2.png)

You’ll see the repository listed in the list of local repositories.
Double click the repository (or click on the blue arrow) to navigate to
the repository.

[![ghfw-nuget-local-repo](http://haacked.com/images/haacked_com/Windows-Live-Writer/0053a500d158_1223E/ghfw-nuget-local-repo_thumb.png "ghfw-nuget-local-repo")](http://haacked.com/images/haacked_com/Windows-Live-Writer/0053a500d158_1223E/ghfw-nuget-local-repo_2.png)

The first time you navigate to the repository, GHfW prompts you for your
credentials to the Git host, in this case, CodePlex.com. This probably
goes without saying, but do not enter your GitHub.com credentials here.

[![ghfw-codeplex-credentials](http://haacked.com/images/haacked_com/Windows-Live-Writer/0053a500d158_1223E/ghfw-codeplex-credentials_thumb.png "ghfw-codeplex-credentials")](http://haacked.com/images/haacked_com/Windows-Live-Writer/0053a500d158_1223E/ghfw-codeplex-credentials_2.png)

GHfW will securely store the credentials for this repository so that you
only need to enter it once. GHfW acts as a credentials provider for Git
so the credentials you enter here will also work with the command line
as long as you launch it from the *Git Shell* shortcut that GHfW
installs. That means you won’t have to enter the credentials every time
you push or pull commits from the server.

With that, you’re all set. Work on your project, make local commits, and
when you’re ready to push your changes to the server, click on the
*sync* button.

[![ghfw-nuget-sync-codeplex](http://haacked.com/images/haacked_com/Windows-Live-Writer/0053a500d158_1223E/ghfw-nuget-sync-codeplex_thumb.png "ghfw-nuget-sync-codeplex")](http://haacked.com/images/haacked_com/Windows-Live-Writer/0053a500d158_1223E/ghfw-nuget-sync-codeplex_2.png)

While we think you’ll have the best experience on GitHub.com, we also
think GitHub for Windows is a great client for any Git host.

Tags: [git](http://haacked.com/tags/git/default.aspx),
[github](http://haacked.com/tags/github/default.aspx),
[ghfw](http://haacked.com/tags/ghfw/default.aspx),
[gh4w](http://haacked.com/tags/gh4w/default.aspx)

