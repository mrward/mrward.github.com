---
layout: page
title: "MonoDevelop NuGet Package Explorer Addin Releases"
date: 2022-02-22 07:00
comments: true
sharing: true
footer: true
---

## Version 0.8

2022-02-22

 * Support VS Mac 17.0.0.7706

# Version 0.7

2021-08-01

 * Support Visual Studio for Mac 17.0

# Version 0.6

2020-11-22

 * Fix Open Package menu not enabled on Solution window in Visual Studio for Mac 8.8

# Version 0.5

2020-05-06

 * Show all non-internal folders in the NuGet package not just known folders

## Version 0.4

2019-08-25

 * Display new license metadata. License expressions now displayed with links to the licenses.
 * Updated to use NuGet 5.2
 * Show package repository information
 * Show package type information (e.g. DotNetCliTool)
 * Show original package version instead of the normalized semantic version. Some packages use a commit hash as the package version which was not being displayed unless the .nuspec tab was selected.

## Version 0.3.3

2019-08-03

 * Fix .nupkg files being open in hex editor

## Version 0.3.2

2019-05-12

 * Support MonoDevelop 8.1.
 * ILMerge Lucene.NET instead of shipping it separately.


## Version 0.3.1

2019-04-11

 * Re-packaged 0.3 to workaround problem with addin server returning old 0.3 addin for MonoDevelop 8.0


## Version 0.3

2018-12-19

 * Support showing SemVer 2.0 NuGet packages in search results.

## Version 0.2

2017-06-07

 * Fixed null reference when opening new file dialog
 * Enable Open Package... menu for .NET Core projects
 * Open dialog if NuGet package cannot be found
 * Support opening a package with .NET Core projects
 * Handle package version wildcards

## Version 0.1

2016-08-13

 * First Release
 