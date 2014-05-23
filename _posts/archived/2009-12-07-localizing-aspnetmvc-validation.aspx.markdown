---
layout: post
title: "Localizing ASP.NET MVC Validation"
date: 2009-12-07 -0800
comments: true
disqus_identifier: 18664
categories: [aspnetmvc]
---
This is the fourth post in my series on ASP.NET MVC 2 and its new
features.

1.  [ASP.NET MVC 2 Beta
    Released](http://haacked.com/archive/2009/11/17/asp.net-mvc-2-beta-released.aspx "Release Announcement")
    (Release Announcement)
2.  [Html.RenderAction and
    Html.Action](http://haacked.com/archive/2009/11/18/aspnetmvc2-render-action.aspx "Html.RenderAction and Html.Action")
3.  [ASP.NET MVC 2 Custom
    Validation](http://haacked.com/archive/2009/11/19/aspnetmvc2-custom-validation.aspx "ASP.NET MVC 2 Custom Validation")
4.  **Localizing ASP.NET MVC Validation**

In my recent post on [custom validation with ASP.NET MVC
2](http://haacked.com/archive/2009/11/19/aspnetmvc2-custom-validation.aspx "Custom Validation with ASP.NET MVC 2"),
several people asked about how to localize validation messages. They
didn’t want their error messages hard-coded as an attribute value.

[![world-in-hands](http://haacked.com/images/haacked_com/WindowsLiveWriter/LocalizingASP.NETMVCValidation_C004/world-in-hands_3.jpg "world-in-hands")](http://www.sxc.hu/browse.phtml?f=view&id=1035531 "Holding Earth 1 by flaivoloka on stock.xchng")It
turns out that it’s pretty easy to do this. Localizing error messages is
not specific to ASP.NET MVC, but is a feature of Data Annotations and
ASP.NET. And everything I cover here works for ASP.NET MVC 1.0 (except
for the part about client validation which is new to ASP.NET MVC 2).

I covered this feature a back in March at Mix 09 in my [ASP.NET MVC
Ninjas on Fire Black Belt
Tips](http://sessions.visitmix.com/MIX09/T44F "Mix Sessions") talk. If
you want to see me walk through it step by step, check out the video. If
you prefer to read about it, continue on!

Let’s start with the `ProductViewModel` I used in the last post

```csharp
public class ProductViewModel {
  [Price(MinPrice = 1.99)]
  public double Price { get; set; }
 
  [Required]
  public string Title { get; set; }
}
```

If we’re going to localize the error messages for the two properties, we
need to add resources to our project. To do this, right click on your
ASP.NET MVC project and select *Properties*. This should bring up the
properties window. Click on the *Resources* tab. You’ll see a message
that says,

> This project does not contain a default resources file. Click here to
> create one.

Obey the message. After you click on the link, you’ll see the resource
editor.

[![resources-tab](/images/haacked_com/WindowsLiveWriter/LocalizingASP.NETMVCValidation_1194E/resources-tab_4.png "resources-tab")](/images/haacked_com/WindowsLiveWriter/LocalizingASP.NETMVCValidation_1194E/resources-tab_4.png)
Make sure to change the *Access Modifier* to *Public*(it defaults to
Internal).

![resources-tab-access-modifier](/images/haacked_com/WindowsLiveWriter/LocalizingASP.NETMVCValidation_1194E/resources-tab-access-modifier_3.png "resources-tab-access-modifier")

Now enter your resource strings in the resource file.

![resource-file](/images/haacked_com/WindowsLiveWriter/LocalizingASP.NETMVCValidation_1194E/resource-file_3.png "resource-file")

Hopefully my Spanish is not too bad. An ASP.NET build provider will
create a new class named `Resources` behind the scenes with a property
per resource string. In this case there’s a property named
`PriceIsNotRight` and `Required`. You can see the new file in the
*Properties* folder of your project.

![solution-with-resources](/images/haacked_com/WindowsLiveWriter/LocalizingASP.NETMVCValidation_1194E/solution-with-resources_3.png "solution-with-resources")

The next step is to annotate the model so that it pulls the error
messages from the resources.

```csharp
public class ProductViewModel {
  [Required(ErrorMessageResourceType = typeof(Resources), 
    ErrorMessageResourceName = "Required")]
  public string Title { get; set; }
  [Price(MinPrice = 3.99, ErrorMessageResourceType = typeof(Resources), 
    ErrorMessageResourceName = "PriceIsNotRight")]
  public double Price { get; set; }
}
```

For the `ErrorMessageResourceType`, I just specify the type created by
the build provider. In my case, the full type name is
`CustomValidationAttributeWeb.Properties.Resources`.

For the `ErrorMessageResourceName`, I just use the name that I specified
in the resource file. The name identifies which resource string to use.

Now when I submit invalid values, the error messages are pulled from the
resource file and you can see they are in Spanish.

[![Client Validation - Windows Internet
Explorer](/images/haacked_com/WindowsLiveWriter/LocalizingASP.NETMVCValidation_1194E/Client%20Validation%20-%20Windows%20Internet%20Explorer_thumb.png "Client Validation - Windows Internet Explorer")](/images/haacked_com/WindowsLiveWriter/LocalizingASP.NETMVCValidation_1194E//images/haacked_com/WindowsLiveWriter/LocalizingASP.NETMVCValidation_1194E/)

### Localized Error Messages Custom Client Validation

Note that these localized error messages continue to work even if you
enable client validation. However, if you were to try it with the
original code I posted in my [last validation
example](http://haacked.com/archive/2009/11/19/aspnetmvc2-custom-validation.aspx "ASP.NET MVC 2 Custom Validation"),
the error message would not work for the custom price validation
attribute.

Turns out I had a bug in the code, which is now corrected in the blog
post with a note describing the fix. Just scroll down to the
`PriceValidator` class.

As always, I have a code sample you can look at. It’s the same example
as before, just updated. **[Download the
sample](http://haacked.com/code/CustomValidationAttributeDemo.zip "Validation Sample App")!**

Tags: [aspnetmvc](http://haacked.com/tags/aspnetmvc/default.aspx),
[asp.net](http://haacked.com/tags/asp.net/default.aspx),
[validation](http://haacked.com/tags/validation/default.aspx),
[localization](http://haacked.com/tags/localization/default.aspx), [data
annotations](http://haacked.com/tags/data+annotations/default.aspx)

