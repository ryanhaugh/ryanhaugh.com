---
layout: post
title: "Supporting STA Threads in Web API"
date: 2014-05-24 -0700
comments: true
categories: [dot-net, web-api]
---

I was reading [this blog post](http://weblog.west-wind.com/posts/2012/Sep/18/Creating-STA-COM-compatible-ASPNET-Applications) about how to support STA threads in ASP.NET and the post has a solution for all ASP.NET technologies except for Web API.  He added a comment saying he couldn't find a way to do it and opened it up for someone else to solve.

Challenge accepted!

But let's back up for a minute...before I describe how to do this, I should explain *why* we'd want to do this.

I recommend reading the aforementioned blog post because Mr. Strahl does a great job describing why this is sometimes required, but I'll provide a quick summary here.

By default, ASP.NET uses Multi-Threaded Apartment (MTA) threads, which are considerably more efficient than Single-Threaded Apartment (STA) threads, meaning we should be using MTA threads as much as possible to avoid incurring the significant overhead of STA threads.

However, in the event that you need to interface with a COM object or, in my client's case, render WPF FixedDocuments on the server side, you're required to force an ASP.NET request into an STA thread so you can successfully interface with objects that are required to stay on a specific thread.

Now that we know the *why*, let's dive into the *how*.

First, we need to come up with a way to create an STA thread.  Since I'm no threading expert, I'm going to defer to people who know a lot more about threading than me: Microsoft.  They were kind enough to create [tons of samples](http://code.msdn.microsoft.com/ParExtSamples/Release/ProjectReleases.aspx?ReleaseId=4179) for the Task Parallel Library.  I'm not as interested in the samples as I am in the additional functionality that they provide with the samples.  They say it's not production quality but I've never had any issues.

The code can be downloaded from this page and manually added to your project.  Or you can choose the more efficient route of searching for a NuGet package that already puts everything together for you.  I chose the latter:

**Install-Package MSFT.ParallelExtensionsExtras**

Now that we have the functionality to easily create STA threads, let's create an extension method to abstract away some boiler-plate:

```csharp 
public static class TaskFactoryExtensions
{
    private static readonly TaskScheduler _staScheduler = new StaTaskScheduler(numberOfThreads: 1);
 
    public static Task<TResult> StartNewSta<TResult>(this TaskFactory factory, Func<TResult> action)
    {
        return factory.StartNew(action, CancellationToken.None, TaskCreationOptions.None, _staScheduler);
    }
}
```

Since we want to utilize STA threads only when absolutely necessary, we need to have a way to determine when an action requires an STA thread:

```csharp
[AttributeUsage(AttributeTargets.Method, AllowMultiple = false, Inherited = true)]
public class UseStaThreadAttribute : Attribute {}
```

Now for the magic - let's create a custom **IHttpActionInvoker**.  Since the default one has most of what we need, we can inherit from that and override the InvokeActionAsync method:

```csharp
public class StaThreadEnabledHttpActionInvoker : ApiControllerActionInvoker
{
    public override Task<HttpResponseMessage> InvokeActionAsync(HttpActionContext context, CancellationToken cancellationToken)
    {
        // Determine whether action has attribute UseStaThread
        bool useStaThread = context.ActionDescriptor.GetCustomAttributes<UseStaThreadAttribute>().Any();
 
        // If it doesn't, simply return the result of the base method
        if (!useStaThread)
        {
            return base.InvokeActionAsync(context, cancellationToken);
        }
 
        // Otherwise, create an STA thread and then call the base method
        Task<HttpResponseMessage> responseTask = Task.Factory.StartNewSta(() => base.InvokeActionAsync(context, cancellationToken).Result);
 
        return responseTask;
    }
}
```

And then we need to configure Web API to use our custom action invoker:

```csharp
// Replaces the default action invoker to allow actions to be run using an STA thread
config.Services.Replace(typeof(IHttpActionInvoker), new StaThreadEnabledHttpActionInvoker());
```

Finally, let's create a sample controller to test it out:

```csharp
public class ApartmentStateController : ApiController
{
    [HttpGet]
    public string Mta()
    {
        return Thread.CurrentThread.GetApartmentState().ToString();
    }
 
    [HttpGet]
    [UseStaThread]
    public string Sta()
    {
        return Thread.CurrentThread.GetApartmentState().ToString();
    }
}
```

That's it!  When you go to **/api/apartmentState/mta** you'll see **MTA** and when you go to **/api/apartmentState/sta** you'll see **STA**.