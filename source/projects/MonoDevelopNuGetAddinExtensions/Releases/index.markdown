---
layout: page
title: "MonoDevelop NuGet Addin Extensions Releases"
date: 2014-06-14 16:30
comments: true
sharing: true
footer: true
---

## Version 0.6

2014-08-30

 * Fix type load exception in the Package Console Extension window in Xamarin Studio 5.3.

## Version 0.5

2014-06-22

 * Implement most of the EnvDTE project model which can be used from the Package Console Extension window.
 * Fix new Package Console not showing projects in drop down if opened after the solution is opened.
 * Standard aliases (e.g. ls, pwd, cd) for commands now defined in Package Console Extension window.
 * Modify addin so it works with MonoDevelop 5.0, 5.1 and 5.2.

## Version 0.4

2014-06-11

 * Add a Package Console Extension window that supports the PowerShell cmdlets Install-Package, Uninstall-Package, Get-Project, Update-Package and Get-Package. Uses [Pash](http://pash-project.github.io/).

## Version 0.3

2014-06-08

 * Allow managing of packages at the solution level. **Manage Packages** menu option available from the **Project** menu and
when the solution is right clicked. This opens the **Manage Packages dialog** allowing packages to be installed into multiple projects
at the same time.

## Version 0.2

2014-06-06

 * Add **List Portable Class Libraries** command to unified search. This new command runs the MonoPcl command line utility which lists the .NET portable class libraries available on the local machine.
 * Rename **Install Package** command in unified search to be **Add Package**.
 * Show package id and version in unified search category.
 * Add new search categories **package** and **nuget** which are a different way to run the **Add Package** command.

## Version 0.1

2014-06-08

 * Support installing a NuGet package from unified search with new **Install Package** command.
 