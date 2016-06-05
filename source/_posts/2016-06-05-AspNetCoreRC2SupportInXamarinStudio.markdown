---
layout: post
title: "ASP.NET Core 1.0 RC2 support in Xamarin Studio"
date: 2016-06-05 19:02
comments: true
categories: NETCore Xamarin MonoDevelop
---

Xamarin Studio and MonoDevelop now have support for [ASP.NET Core 1.0 RC2](https://blogs.msdn.microsoft.com/webdev/2016/05/16/announcing-asp-net-core-rc2/) with an alpha release of the [.NET Core addin](https://github.com/mrward/monodevelop-dnx-addin).

{% img /images/blog/AspNetCoreRC2SupportInXamarinStudio/WebProjectInSolutionWindow.png 'ASP.NET Core web project in Solution window' 'ASP.NET Core web project in Solution window' %}

This is an update of the original [DNX addin](/blog/2016/01/01/DNXSupportInXamarinStudio/) which adds support for .NET Core RC2 and also, thanks to [David Karlaš](twitter.com/davidkarlas), adds support for debugging .NET Core applications on the Mac.

## Features

   * Debugging .NET Core applications with the .NET Core CLR on Mac.
   * Project templates for console, library and web applications
   * Code completion
   * NuGet integration
   * Solution window integration

## Supports 

   * MonoDevelop and Xamarin Studio 6.0.
   * [.NET Core 1.0 RC2](https://www.microsoft.com/net/core).

## Installing .NET Core SDK

The [.NET Core SDK](https://www.microsoft.com/net/core) needs to be installed separately before using the .NET Core addin. Detailed installation instructions can be found on [Microsoft's .NET Core web site](https://www.microsoft.com/net/core).

## Installing the addin

The addin is currently available from [MonoDevelop's Add-in Repository](http://addins.monodevelop.com/) in the alpha channel. In Xamarin Studio open the Add-in Manager and select the Gallery tab. Click the repository drop down and if **Xamarin Studio Add-in Repository (Alpha Channel)** is not displayed then click Manage Repositories. In the window that opens tick the check box next to Xamarin Studio Add-in Repository (Alpha Channel) and then click the Close button.

{% img /images/blog/AspNetCoreRC2SupportInXamarinStudio/AddinManagerNetCoreAddin.png '.NET Core addin selected in Addin Manager dialog' '.NET Core addin selected in Addin Manager dialog' %}

Select the .NET Core addin and then click the Install button.

After installing the .NET Core addin you will need to restart Xamarin Studio before the project templates are available in the New Project dialog.

## Creating a .NET Core project

There are three project templates available for .NET Core in the New Project dialog.

{% img /images/blog/AspNetCoreRC2SupportInXamarinStudio/NewProjectDialogNetCoreProjectTemplates.png 'New ASP.NET 5 project templates' 'New ASP.NET 5 project templates' %}

## Debugging

Thanks to [David Karlaš](https://twitter.com/davidkarlas) there is a support for debugging .NET Core applications when running on the .NET Core CLR if you have the [VSCode Debugger addin](http://addins.monodevelop.com/Project/Index/228) installed.

{% img /images/blog/AspNetCoreRC2SupportInXamarinStudio/DebuggingNetCoreApplication.png 'Debugging a .NET Core application' 'Debugging a .NET Core application' %}

The VSCode Debugger addin is currently available from [MonoDevelop's Add-in Repository](http://addins.monodevelop.com/) on the alpha channel.

Debugging .NET Core console and web projects that target the full .NET framework is supported on all platforms. The .NET Core command line tool (dotnet.exe) will create an executable when targeting the full .NET Framework which can be debugged in Xamarin Studio. On Windows the x86 version of the .NET Core SDK needs be installed since Xamarin Studio currently cannot debug x64 applications on Windows.

## Source Code

The source code for the addin is available on GitHub.

  - [.NET Core addin source code](https://github.com/mrward/monodevelop-dnx-addin).
