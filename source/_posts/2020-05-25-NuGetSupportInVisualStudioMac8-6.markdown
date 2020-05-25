---
layout: post
title: "NuGet Support in Visual Studio for Mac 8.6"
date: 2020-05-25 10:00
comments: true
categories: NuGet Xamarin VSMac MonoDevelop
---

## Changes

   * NuGet 5.6 support
   * Fixed escape key not closing Manage NuGet Packages dialog
 
More information on all the new features and changes in [Visual Studio for Mac 8.6](https://www.visualstudio.com/vs/visual-studio-mac/)
can be found in the [release notes](https://docs.microsoft.com/en-us/visualstudio/releasenotes/vs2019-mac-relnotes).

## NuGet 5.6 support
    
[NuGet 5.6.0.6591](https://docs.microsoft.com/en-us/nuget/release-notes/nuget-5.6) is now
included with Visual Studio for Mac 8.6.

## Bug Fixes

**Fixed escape key not closing Manage NuGet Packages dialog**

When the search text box had focus pressing the escape key would
not close the Manage NuGet Packages dialog as it did in previous
versions. A change to fix accessibility of the search text box
broke the original behaviour of the dialog.
