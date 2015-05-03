---
layout: post
title: "NuGet Support in Xamarin Studio 5.9"
date: 2015-05-03 14:00
comments: true
categories: NuGet Xamarin MonoDevelop
---

## Changes

   * NuGet 2.8.3 support
   * Always show Packages folder in Solution window
   * Target framework change detected on project reload

More information on all the new features and changes in Xamarin Studio 5.9 can be found in the [release notes](http://developer.xamarin.com/releases/studio/xamarin.studio_5.9/xamarin.studio_5.9/).

## NuGet 2.8.3 support

Xamarin Studio now supports NuGet 2.8.3. This allows a NuGet package to target NuGet 2.8.3 explicitly. For example the PCLStorage 1.0.1 NuGet package will not install into Xamarin Studio 5.8, since it requires NuGet 2.8.3, but will install into Xamarin Studio 5.9.

NuGet packages, such as xunit, that target the new ASP.NET target frameworks, ASP.NetCore 5.0 and ASP.Net 5.0, can now be installed into Xamarin Studio now that it supports NuGet 2.8.3. Previously you would see an error message in the Package Console window:

    'xunit.core' already has a dependency defined for 'xunit.extensibility.core'.

Support for NuGet 2.8.5 is planned for Xamarin Studio 5.9.1.

## Always Show Packages Folder in Solution window

The Packages folder is now always shown in the Solution window even if the project has no NuGet packages. Previously the Packages folder would only be shown if one or more NuGet packages were installed in a project.

{% img /images/blog/NuGetSupportInXamarinStudio5-9/PackagesFolderInSolutionWindow.png 'Packages folder in Solution window' 'Packages folder in Solution window' '' %}

## Target Framework Change Detected on Project Reload

Xamarin Studio will detect a project file has been changed outside of Xamarin Studio and will reload the project. Now Xamarin Studio on reloading will detect the project's target framework has been changed and will  check the NuGet packages are compatible with the new target framework. Previously Xamarin Studio would only check the compatibility of NuGet packages if the target framework was changed from within Xamarin Studio via the project options.

This allows Xamarin Studio to check the NuGet packages are compatible when an iOS Classic project is converted to an iOS Unified project using Xamarin Studio's migration tool. The NuGet packages, such as Xamarin.Forms, can then be retargeted by Xamarin Studio using the [Retarget menu](blog/2014/08/10/NuGetSupportInXamarinStudio5-2/).
