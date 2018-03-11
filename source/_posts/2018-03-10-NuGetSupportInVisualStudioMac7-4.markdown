---
layout: post
title: "NuGet Support in Visual Studio for Mac 7.4"
date: 2018-03-10 12:00
comments: true
categories: NuGet Xamarin VSMac MonoDevelop
---

## Changes

   * Support NuGet RestoreProjectStyle MSBuild Property
   * .NET Core xUnit tests not displayed if NuGet packages not cached
   * No tests displayed when project uses a NUnit PackageReference
   * NuGet MSBuild imports not removed when migrating to project.json

More information on all the new features and changes in [Visual Studio for Mac 7.4](https://www.visualstudio.com/vs/visual-studio-mac/)
can be found in the [release notes](https://docs.microsoft.com/en-us/visualstudio/releasenotes/vs2017-mac-relnotes#15.6).

## Support RestoreProjectStyle MSBuild property
    
If a project sets the RestoreProjectStyle property to be
PackageReference then the project will be restored as though it has
PackageReferences even if it does not have any.
    
    <RestoreProjectStyle>PackageReference</RestoreProjectStyle>
    
This fixes runtime problems when a project references a .NET Standard
project that has PackageReferences. Without the RestoreProjectStyle set
the assemblies from the NuGet packages are not copied to the output 
directory and would have to be added to the main project.

## Bug Fixes

**.NET Core xUnit tests not displayed if NuGet packages are not in local NuGet cache**

If the NuGet packages that contain the VS Test adapters were not available in
the local machine's NuGet package cache then
the Unit Tests window would not show any tests after the NuGet
packages were restored and the project was compiled.

Visual Studio for Mac would not initially find any test adapter dlls
and would not attempt to discover any tests for the project after the
NuGet packages were downloaded into the NuGet package cache.

Now after the NuGet package restore has completed the check for a
VS Test adapter is now re-run.

**No tests displayed when a project uses a NUnit PackageReference**

The Unit Tests window would not show any unit tests when a non .NET
Core project contained a NUnit PackageReference. Visual Studio for
Mac was looking for a PackageReference that contained 'nunit.framework'
and was not finding the NUnit NuGet package reference.

**NuGet MSBuild imports not removed when migrating to project.json**

When a Portable Class Library project was migrated to use a project.json
file the MSBuild imports added by NuGet were not removed. These imports 
are added by NuGet into the generated ProjectName.nuget.props and
ProjectName.nuget.targets files on restoring a project.json file. 
Leaving the import in the project file would result in the import
being used twice. When a PCL project that used Xamarin.Forms was
migrated to project.json the project would fail to compile with the
error:
    
    Error XF001: Xamarin.Forms targets have been imported multiple times.
    Please check your project file and remove the duplicate import(s).
    
Now on migrating to project.json the MSBuild imports added by NuGet packages
to the project are removed.

**Fix unhandled exception when searching for packages**

An unhandled exception was being logged if a package source returned
an error or the package source url was invalid. This was because a
task was not being observed when the package sources were being used.