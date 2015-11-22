---
layout: post
title: "NuGet Support in Xamarin Studio 5.10"
date: 2015-11-22 12:00
comments: true
categories: NuGet Xamarin MonoDevelop
---

Xamarin Studio 5.10 was released last week as part of the [Xamarin 4 release](https://blog.xamarin.com/introducing-xamarin-4/) and it includes changes to the NuGet support.

## Changes

   * Support NuGet 2.8.7.
   * Open readme.txt when a NuGet package is installed.
   * Support packages.config file named after the project.
   * Local Copy settings are preserved for references when updating packages.
   * Do not show Checking for package updates message in status bar.
   * Do not show warning in the status bar if a NuGet package has PowerShell scripts.
   * Prevent the solution being closed when NuGet packages are being added.
   * Removing a NuGet package does not update the Solution window when multiple solutions are open.
   * Prevent packages.config file being marked as deleted by Git after updating a pre-release NuGet package.
   * Prevent retargeting a NuGet package marking packages.config as deleted by Git.
   * Allow Microsoft.ApplicationInsights NuGet package to be installed.

More information on all the changes in Xamarin Studio 5.10 can be found in the [release notes](http://developer.xamarin.com/releases/studio/xamarin.studio_5.10/xamarin.studio_5.10/).

## NuGet 2.8.7 support

Xamarin Studio now supports NuGet 2.8.7. NuGet 2.8.7 adds support for the Universal App Platform (UAP) target framework to support Windows 10 Application Development.

## Open readme.txt when a NuGet package is installed

A NuGet package can contain a [readme.txt file](https://docs.nuget.org/create/creating-and-publishing-a-package#user-content-automatically-displaying-a-readmetxt-file-during-package-installation) which Xamarin Studio will now open and display in the text editor when the NuGet package is installed or updated.

## Preserve Local Copy on Updating Packages

The Local Copy setting on an assembly reference will now be preserved when updating a NuGet package or retargeting a NuGet package.

By default Local Copy is set to true for assembly references when installing a NuGet package. If you set Local Copy to false for one or more of these references then this setting will now be preserved when updating or retargeting the NuGet package.

## Packages.config file named after project

NuGet supports multiple projects in the same directory each using their own packages.config file. To allow multiple projects in the same directory to each use their own NuGet packages you can name the packages.config file after each project. In the examples below the project filename is on the left and the corresponding packages.config filename is on the right.

 * Foo.csproj => packages.Foo.config
 * Bar.csproj => packages.Bar.config
 * Foo Bar.csproj => packages.Foo_Bar.config

Xamarin Studio now checks for the packages.ProjectName.config file first and will use it if it exists, otherwise it will fall back to the default behaviour and use the packages.config file.

Note that a new project without any NuGet packages will use a packages.config file by default. The basic procedure to enable a project specific packages.config file when creating a new project is:

 1. Create new project called Foo.
 2. Add a NuGet package to the Foo project.
 3. Rename the packages.config file to packages.Foo.config
 4. Reload the solution in Xamarin Studio.
 
Also note that if you remove all the NuGet packages from a project the packages.ProjectName.config file will be deleted and on adding a new NuGet package the default packages.config file will be used.

## Do not show Checking for package updates message in status bar

Previously when Xamarin Studio was checking for NuGet package updates a message would appear in the status bar. This status bar message has now been removed since checking for NuGet package updates is a background task and does not prevent Xamarin Studio from being used.

## Do not show warning in the status bar if a NuGet package has PowerShell scripts

Previously if a NuGet package was installed and it contained PowerShell scripts then a warning was shown in the status bar. Now this message is only shown in the Package Console window.

## Prevent the solution being closed when NuGet packages are being added

A check is now made when Xamarin Studio is closed to see if NuGet packages are still being installed. If this is the case then a dialog will be displayed indicating that it is not currently possible to close Xamarin Studio allowing the NuGet package to finish installing.

## Bug Fixes

**Removing a NuGet package does not update the Solution window when multiple solutions are open**

With two or more solutions opened at the same time the Packages folder would not be updated for all solutions when a NuGet package was removed. This was because Xamarin Studio was not refreshing the Packages folder for all solutions currently open.

**Prevent packages.config file being marked as deleted by Git after updating pre-release NuGet package.**

If there was only one pre-release NuGet package installed into a project and then a later version of the NuGet package was installed from the Add Packages dialog then the packages.config file was then being shown as deleted by Git instead of modified.

The packages.config file is deleted by NuGet after the old NuGet package is uninstalled if there are no NuGet packages referenced. A special case to handle this was added in Xamarin Studio 5.3 but that only handled updating a NuGet package from the Solution window. Now updating a pre-release from the Add Packages dialog is also handled.

**Retargeting a NuGet package marks packages.config as deleted by Git**

This is similar to the previous bug. Retargeting a NuGet package will uninstall and then install the NuGet package. If there is only one NuGet package in the project then the packages.config file is deleted and was causing Git to mark the file as deleted instead of updated.

**Unable to install Microsoft.ApplicationInsights NuGet package**

Xamarin Studio 5.9.2 added support for NuGet 2.8.5 but it was not possible to install the [Microsoft.ApplicationInsights NuGet package](https://www.nuget.org/packages/Microsoft.ApplicationInsights/) into a project. It was possible to install it using Visual Studio with NuGet 2.8.3 or higher installed. The error reported by Xamarin Studio was:

    Adding Microsoft.ApplicationInsights...
    The 'Microsoft.ApplicationInsights' package requires NuGet client version '2.8.50313' or above, but the current NuGet version is '2.8.5.0'.

Xamarin Studio 5.10 now allows the Microsoft.ApplicationInsights NuGet package to be installed into a project.
