---
layout: page
title: "MonoDevelop NuGet Addin 0.8"
date: 2013-12-15 18:22
comments: true
sharing: true
footer: true
---

## New Features

* [NuGet 2.7.2](http://docs.nuget.org/docs/release-notes/nuget-2.7.2) support.

### NuGet 2.7.2 and PCL Support

With the upgrade to NuGet 2.7.2 the Portable Class Library support has been improved further. You can now add a NuGet package, containing a PCL, such as [Json.NET](https://www.nuget.org/packages/Newtonsoft.Json/), to a project that targets MonoAndroid. Previous versions of the addin would fail with an error message indicating that it was not possible to install Json.NET into a project targeting MonoAndroid.

## Bug Fixes

* Fix old NuGet packages showing as installed in solution when they are not installed.
* Fix old NuGet packages not being removed on updating to a new package.
* Fix exception when NuGet feed results returned after the Manage Packages dialog is closed.

## Download

The source code for the addin is available on [GitHub](https://github.com/mrward/monodevelop-nuget-addin).

The addin is also available to download from a custom MonoDevelop repository:

* [Xamarin Studio 4.0 and 4.2](http://mrward.github.com/monodevelop-nuget-addin-repository/4.0/main.mrep)
* [MonoDevelop 3.0](http://mrward.github.com/monodevelop-nuget-addin-repository/3.0.5/main.mrep)
