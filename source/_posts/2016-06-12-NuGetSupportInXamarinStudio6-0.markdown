---
layout: post
title: "NuGet Support in Xamarin Studio 6.0"
date: 2016-06-12 14:00
comments: true
categories: NuGet Xamarin MonoDevelop
---

Xamarin Studio 6.0 was released last week and it includes a lot of [new features](https://blog.xamarin.com/live-from-dotnetconf-cycle-7-xamarin-studio-6-and-more/), such as a new dark theme and [Roslyn integration](https://github.com/dotnet/roslyn). This release also includes some improvements made to the NuGet support.

{% img /images/blog/NuGetSupportInXamarinStudio6-0/AddPackagesDialogDarkTheme.png 'Add Packages Dialog - Dark Theme' 'Add Packages Dialog - Dark Theme' %}  

## New Features

   * Support NuGet packages targeting tvOS.
   * Support updating pre-release NuGet packages.
   * Show updates available for pre-release NuGet packages.
   * Remember Show pre-release Packages setting in Add Packages dialog.
   * Error dialog displayed if NuGet.Config file cannot be read.

More information on all the changes in Xamarin Studio 6.0 can be found in the [release notes](https://developer.xamarin.com/releases/studio/xamarin.studio_6.0/xamarin.studio_6.0/).

## Support NuGet packages targeting watchOS

A new Xamarin.WatchOS target framework is now supported which allows NuGet packages to contain assemblies for watchOS.

##  Support updating pre-release NuGet packages

Previously it was not possible to update a pre-release NuGet package to a later pre-release  from the Solution window, only updates to stable NuGet packages were supported. The only way to update to a later pre-release NuGet package was to use the Add Packages dialog.

Now an individual pre-release NuGet package can be updated by right clicking and selecting Update. When all packages in a project or solution are updated then pre-release NuGet packages will be updated to a later pre-release version if they are available.

## Show updates available for pre-release NuGet packages

Previously Xamarin Studio would only show stable NuGet package updates as being available if a pre-release NuGet package was installed. Now Xamarin Studio will check for updates for pre-release NuGet packages as well as stable packages and display this information in the Solution window.

{% img /images/blog/NuGetSupportInXamarinStudio6-0/PreReleaseNuGetPackageUpdatesInSolutionWindow.png 'Pre-release NuGet package updates in Solution window' 'Pre-release NuGet package updates in Solution window' %} 

Only if an installed NuGet package is a pre-release version will pre-release updates be shown as available in the Solution window. Xamarin Studio will not check for pre-release updates for stable NuGet package versions that are installed.

## Remember Show pre-release Packages setting in Add Packages dialog

The Show pre-release Packages check box setting will now be remembered in the Add Packages dialog on a per solution basis.

## Error dialog displayed if the NuGet.Config file cannot be read

Previously if the NuGet.Config file could not be read the error would
be silently logged, but not reported, and Xamarin Studio would
then switch to using the default official NuGet package source. Now an error
dialog is shown indicating that there was a problem reading the
NuGet.Config file.

## Bug Fixes

**Support NuGet packages that use icons from local files**

A NuGet package can now use an icon, which will be shown in the Add Packages
dialog, taken from the local file system using a file url. Previously this would fail with an invalid cast exception.

**Incorrect update count displayed after updating NuGet packages.**

When an update caused a NuGet package to be uninstalled the Packages
folder in the Solution window would show an incorrect count for the available updates.

**NuGet restore and update not working for workspaces**

With a workspace opened, or multiple solutions opened in Xamarin Studio, then restoring and updating NuGet packages would only work for one of the solutions.

**Unable to add Google Play Services packages**

The Xamarin.Android.Support.v7.AppCompat NuGet package depends on
a single version of the Xamarin.Android.Support.v4 NuGet package.
When a Xamarin Google Play Services NuGet package is installed
that depends on a higher version of the Xamarin.Android.Support.v4
NuGet package the install would fail to find a version of the
AppCompat NuGet package that is compatible. An error similar to the
following would be displayed in the Package Console window:

    Could not add Xamarin.GooglePlayServices.Ads.
    Updating 'Xamarin.Android.Support.v4 23.1.1.0' to
    'Xamarin.Android.Support.v4 23.1.1.1' failed. Unable to find a version
    of 'Xamarin.Android.Support.v7.AppCompat' that is compatible with
    'Xamarin.Android.Support.v4 23.1.1.1'.

The problem was that the AppCompat NuGet package is not involved
in the initial NuGet package resolution using the remote package
source so it is initially not considered for installation into the
packages directory. When a package reference is then added to the
packages.config file a search for a compatible AppCompat package was
only using the local solution's packages directory which fails causing
the install to fail.

To fix this the local solution packages directory is used first
when looking for a compatible AppCompat NuGet package and will
fallback to using the configured remote package sources if no suitable package
is found in this directory. To handle a package being added to the
packages.config at this point, after the original NuGet packages have
already been downloaded to the solution's packages directory, Xamarin Studio will detect a package is added to the packages.config file and install the NuGet package if it is not
already in the local solution packages directory.

**Incorrect package version being installed**

When installing a NuGet package using the Google Play Services dialog a package version is not specified by the dialog. If the NuGet package being installed was found in the local machine's NuGet cache it would be used instead of the latest version from the official NuGet gallery at nuget.org. This could result in a lower version being installed than expected.

**MSBuild property files (.props) not being added at correct project location**

Installing a NuGet package that included an MSBuild .props file would
add an Import element for the .props at the end of the project file
(.csproj) instead of at the start. The .props files are now added to the project 
file as the first child element inside the project's root element.