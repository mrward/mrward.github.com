---
layout: page
title: "MonoDevelop NuGet Addin Extensions Releases"
date: 2015-06-28 13:24
comments: true
sharing: true
footer: true
---

## Version 0.9

2015-06-28

 * Allow solution level packages to be uninstalled from console.
 * Fix argument cannot be null message when uninstalling unknown package from console.
 * Support version constraints in packages.config file when using Update-Package in console.

## Version 0.8

2015-05-30

 * Update to NuGet 2.8.5 to support Xamarin Studio 5.9.2
 * Fix gtk critical errors when opening Manage Packages dialog.

## Version 0.7

2015-05-17

 * Fix exception when import removed when unintalling AutoMapper. 
 * Support PowerShell script that add imports to MSBuild global projects. 
 * Support target framework specific NuGet PowerShell scripts. 
 * Fix custom repository paths not used for Install-Package cmdlet.
 * Colour text in Package Console Extension window. 
 * Support -verbose parameter in PowerShell console. 

## Version 0.6.1

2015-04-21

 * Update to NuGet 2.8.3 to support Xamarin Studio 5.9
 
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
 