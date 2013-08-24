---
layout: post
title: "MonoDevelop NuGet Addin 0.5 Released"
date: 2013-07-14 15:40
comments: true
categories: NuGet Xamarin MonoDevelop
---

A new version of the NuGet addin for Xamarin Studio 4.0 has been released.

## New Features

* [NuGet 2.6](http://docs.nuget.org/docs/release-notes/nuget-2.6) support.
* Support for prerelease packages.
* Support for packages using [XML Document Transformations](https://xdt.codeplex.com/) (XDTs).
* File conflict dialog when installing a package that is trying to add files that already exist in the project.
* Installation errors now displayed in the Manage Packages dialog.
* Support for packages that include MSBuild targets and properties files.
* Support for accessing authenticated feeds.
* Update All button added to Manage Packages dialog so all packages can be updated in a project or solution in one step.
* Package title displayed in the list of packages instead of the package id. The package id displayed when a package is selected on the right hand side of the dialog.

For a detailed look at the new features please read the [release note](/projects/MonoDevelopNuGetAddin/Releases/0.5/).