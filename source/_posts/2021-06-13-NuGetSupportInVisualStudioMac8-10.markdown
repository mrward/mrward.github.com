---
layout: post
title: "NuGet Support in Visual Studio for Mac 8.10"
date: 2021-06-13 11:30
comments: true
categories: NuGet Xamarin VSMac MonoDevelop
---

## Changes

   * Manage NuGet Packages dialog now uses native macOS UI
   * NuGet 5.9 support
   * Support disabling NuGet restore in NuGet.Config
   * Fixed automatic restore may still run if disabled in preferences
   * Fixed $(SolutionDir) not defined when restoring large solutio
 
More information on all the new features and changes in [Visual Studio for Mac 8.10](https://www.visualstudio.com/vs/visual-studio-mac/)
can be found in the [release notes](https://docs.microsoft.com/en-us/visualstudio/releasenotes/vs2019-mac-relnotes).

## Manage NuGet Packages dialog now uses native macOS UI

The Manage NuGet Packages dialog is now displayed using Xamarin.Mac.

{% img /images/blog/NuGetSupportInVisualStudioMac8-10/ManageNuGetPackagesDialog.png 'Manage NuGet packages dialog' 'Manage NuGet packages dialog' %}

Previously the dialog was displayed using GTK#.

The following UI changes were also made:

 - Tabs are now used for each page
   - Previously a link was used to select a page
 - Search has moved to the same line as the window title
 - NuGet package sources have moved to the bottom
 of the dialog
 - `Show pre-release packages` check box has been renamed to `Include prereleases`
 - Hyperlinks that were used to view the license and project pages
 have been replaced with buttons
 - Close button has been removed

## NuGet 5.9 support
    
[NuGet 5.9.0.7134](https://docs.microsoft.com/en-us/nuget/release-notes/nuget-5.9) is now
included with Visual Studio for Mac 8.10.

## Support disabling NuGet restore in NuGet.Config

A NuGet.Config file can disable package restore via a enabled setting, as shown below.

    <configuration>
      <packageRestore>
        <add key="enabled" value="false" />
      </packageRestore>
    </configuration>

This is now supported in Visual Studio for Mac and will disable automatic package
restore overriding the setting in Preferences.

**Fixed automatic restore may still run if disabled in preferences**

Certain actions in SDK style projects, such as saving a project file in the
editor, and adding a reference to another project, would run a package restore even
if automatic restore was disabled in preferences.

Now, if automatic restore is disabled in preferences, no automatic restore
will be run. Restores can still be run from the main menu and the
Solution window.

**Fixed $(SolutionDir) not defined when restoring large solution**

When a solution contains more than 10 projects then MSBuild is used
directly to restore the projects. If a project was using the SolutionDir
property to import an MSBuild file then this would fail since SolutionDir
is not defined because no solution was being passed to MSBuild.

One way to reproduce this was to have an SDK style project that does not
define a TargetFramework itself but conditionally imports a project,
using a path that includes the $(SolutionDir) property, that defines
the TargetFramework. The NuGet restore would fail with an error about an
invalid framework identifier.

    error MSB4018: The “WriteRestoreGraphTask” task failed unexpectedly.
    error MSB4018: NuGet.Frameworks.FrameworkException: Invalid
        framework identifier ‘’.

To fix this Visual Studio for Mac now runs the GenerateRestoreGraphFile
target specifying the solution, not just the projects. This allows
MSBuild to define the $(SolutionDir) property.

