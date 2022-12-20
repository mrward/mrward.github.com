---
layout: post
title: "NuGet Support in Visual Studio for Mac 17.4"
date: 2022-12-20 12:20
comments: true
categories: NuGet Xamarin VSMac MonoDevelop
---

## Changes

   * NuGet 6.3.1 support
   * Fixed multiple NuGet.Config load error dialogs
   * Fixed wrong NuGet.Config file path when failing to save file
 
More information on all the new features and changes in [Visual Studio for Mac 17.4](https://visualstudio.microsoft.com/vs/mac/)
can be found in the [release notes](https://learn.microsoft.com/visualstudio/releases/2022/mac-release-notes).

## NuGet 6.3.1 support
    
[NuGet 6.3.1.1](https://learn.microsoft.com/en-us/nuget/release-notes/nuget-6.3) is now
included with Visual Studio for Mac 17.4.

**Fixed multiple NuGet.Config load error dialogs**

On opening or creating a new solution, and the global NuGet.Config file was invalid,
two error dialogs would be displayed indicating
a problem with the NuGet.Config file.

Now the error dialog is only displayed once
when opening a solution.

**Fixed wrong NuGet.Config file path when failing to save file**

When there is an error saving the NuGet.Config the error message
shows the path to the file. However this was using SpecialFolder.ApplicationData
instead of the correct path `~/.nuget/NuGet/NuGet.config`.
