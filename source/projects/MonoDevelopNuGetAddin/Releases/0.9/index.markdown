---
layout: page
title: "MonoDevelop NuGet Addin 0.9"
date: 2014-04-13 14:54
comments: true
sharing: true
footer: true
---

## New Features

* [NuGet 2.8.1](http://docs.nuget.org/docs/release-notes/nuget-2.8.1) support.

## NuGet 2.8.1

NuGet 2.8.1 includes support for Windows Phone 8.1 projects with the new target frameworks:

* WindowsPhone81 / WP81 (for Silverlight-based Windows Phone projects)
* WindowsPhoneApp81 / WPA81 (for WinRT-based Windows Phone App projects)

## Bug Fixes

* Fix target framework for dependency being ignored.

A NuGet package can specify a dependency on another NuGet package when
a particular target framework is being installed:

    <dependencies>
       <group targetFramework="net40">
          <dependency id="MyDependency" version="1.0"/>
       </group>
    </dependencies>

Installing this NuGet package into a project that targets net20, with
the MyDependency NuGet package being unavailable on any NuGet feed, will now no longer fail.
Only when installing the NuGet package into a project that targets net40
will this dependency be resolved.

## Download

The source code for the addin is available on [GitHub](https://github.com/mrward/monodevelop-nuget-addin).

The addin is also available to download from a custom MonoDevelop repository:

* [Xamarin Studio 4.x](http://mrward.github.com/monodevelop-nuget-addin-repository/4.0/main.mrep)
