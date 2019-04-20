---
layout: post
title: "NuGet Support in Visual Studio for Mac 8.0"
date: 2019-04-20 12:40
comments: true
categories: NuGet Xamarin VSMac MonoDevelop
---

## Changes

   * Fixed build action not available after installing package into a PackageReference project
   * Fixed NuGet extension api install events not raised for PackageReference projects

More information on all the new features and changes in [Visual Studio for Mac 8.0](https://www.visualstudio.com/vs/visual-studio-mac/)
can be found in the [release notes](https://docs.microsoft.com/en-us/visualstudio/releasenotes/vs2019-mac-relnotes).

## Bug Fixes

**Fixed build action not available on installing package into a PackageReference project**

After installing the Xamarin.GooglePlayServices.Basement NuGet package, into a project
that used PackageReferences, the GoogleServicesJson build action, defined by this NuGet package,
was not available in the list of build actions when you right clicked a file in the
Solution window. The build action was available after the solution was closed and re-opened.

Installing a NuGet package into a project that used PackageReferences
would not re-evaluate the project's MSBuild information. This resulted in any custom
[AvailableItemNames](https://docs.microsoft.com/en-us/visualstudio/msbuild/visual-studio-integration-msbuild?view=vs-2019#additional-build-actions) 
not being available to be used as a build action
in the Solution window. The build actions for a project were cached so these
are now cleared to ensure the latest items are available
after an re-evaluation.

**Fixed NuGet extension api install events not raised for PackageReference projects**

The NuGet extension API has a PackageReferenceAdded event and a
PackageReferenceRemoved. These were being raised if a project had
a packages.config file but not if the project used PackageReferences.

