---
layout: post
title: "NuGet Support in Xamarin Studio 5.3"
date: 2014-09-07 10:00
comments: true
categories: NuGet Xamarin MonoDevelop
---

## Changes

   * NuGet package restore is now part of Xamarin Studio and no longer uses NuGet.exe
   * Add Packages dialog - Package sources could not be reached shown for failing package sources
   * Packages restored for the selected project instead of the solution
   * Show packages added to solution in Add Packages dialog
   * Show packages up to date message in status bar if there are no package updates available
   * Show status bar warning message when no updates found and package sources are unavailable
   * Restore missing packages before updating a package
   * Do not check for updated packages if the project has no packages
   * Fix version shown as download count in Add Packages dialog when searching for package versions
   * Fix empty source being selected in Add Packages dialog when package source disabled and All Sources selected
   * Fix packages.config marked as deleted by Git when updating packages
   
More information on all the new features and changes in Xamarin Studio 5.3 can be found in the [release notes](http://developer.xamarin.com/releases/studio/xamarin.studio_5.3/xamarin.studio_5.3/). Now let us take a more detailed look at the new NuGet changes.

## NuGet package restore no longer uses NuGet.exe

The NuGet package restore is now a part of Xamarin Studio and no longer uses NuGet.exe. This allows the package restore to integrate with the Xamarin Studio credential provider and provides more control over the package restore process. So if a package source needs authentication, or uses a proxy, then Xamarin Studio will now show a dialog asking for credentials if the credentials are not stored. Previously the package restore would fail with an error message logged in the Package Console.
   
## Add Packages dialog - Package sources could not be reached 

Previously when All Sources was selected and if any package source could not be reached then an error message would be displayed and no packages would be shown in the list. Now packages will be displayed with a warning even if one package source could not be reached.

{% img /images/blog/NuGetSupportInXamarinStudio5-3/AddPackagesDialogPackageSourcesCouldNotBeReachedWarning.png 'Add Packages dialog - package sources could not be reached warning' 'Add Packages dialog - package sources could not be reached warning' %}

## Packages restored for the selected project instead of the solution

Xamarin Studio will now restore packages for the selected project instead of the entire solution. When you right click the **Packages** folder in the Solution window and select **Restore** only the packages for that project will be restored.

{% img /images/blog/NuGetSupportInXamarinStudio5-0/SolutionWindowRestorePackagesMenu.png 'Packages folder - Restore menu' 'Packages folder - Restore menu' %}

This is now possible since Xamarin Studio is now responsible for restoring packages instead of using NuGet.exe which would only restore for the entire solution.

To restore packages for the entire solution you can still use the **Restore Packages** menu which is available from the Project menu or by right clicking the solution in the Solution window.

## Show packages added to solution in Add Packages dialog

Opening the Add Packages dialog will now show the packages added to all projects in the current solution.

The order of the items displayed in the Add Packages dialog is recent packages first, then solution packages, and then the packages from the active package source.

## Show packages up to date message in status bar if there are no package updates available

When you try to update a package and there are no package updates available then the status bar now displays a message indicating that the package is already up to date.

{% img /images/blog/NuGetSupportInXamarinStudio5-3/PackageUpToDateStatusBarMessage.png 'Package up to date status bar message' 'Package up to date status bar message' %}

Similarly if you update multiple packages and there are no updates available then the status bar will now show a packages are up to date message.

{% img /images/blog/NuGetSupportInXamarinStudio5-3/PackagesAreUpToDateStatusBarMessage.png 'Packages are up to date status bar message' 'Packages are up to date status bar message' %}

Previously the status bar would show a message that the package was updated successfully even if nothing was updated.

## Show status bar warning message when no updates found and package sources are unavailable

When one or more of the package sources is unavailable or invalid then Xamarin Studio will now report a warning in the status bar after checking for updates. 

{% img /images/blog/NuGetSupportInXamarinStudio5-3/NoUpdateFoundButWarningsReportedStatusBarMessage.png 'No update but warnings reported status bar message' 'No updated found but warnings reported status bar message' %}

## Restoring missing packages before updating packages

Previously when NuGet packages were unrestored and an attempt was made to update a NuGet package, which had updates available from the package source, the update would fail with a message indicating that the package was installed successfully but the project did not reference the package.

To prevent the update from failing Xamarin Studio will now check that the packages are restored for the project before trying to update and restore any missing packages. In the status bar a **Restoring packages before update** message will be displayed when a restore must be completed first.

{% img /images/blog/NuGetSupportInXamarinStudio5-3/RestoringPackagesBeforeUpdateStatusBarMessage.png 'Restoring packages before update status bar message' 'Restoring packages before update status bar message' %}

## Bug Fixes

**Do not check for updated packages if the project has no packages**

Xamarin Studio was checking for package updates in all projects even if they had no packages.config file when the solution was opened. This would result in the **Packages are up to date** message being displayed in the status bar even when no projects were using any NuGet packages.

Now if the project has no packages.config file then Xamarin Studio will not check for updates.

**Fix version shown as download count in Add Packages dialog when searching for package versions**

When running a package version search, such as **Xamarin.Forms version:***, the right hand side of the dialog was showing the version number instead of the download count.

Now the dialog shows the download count. Ideally it would show the download count of that particular version but this is not currently available from the Package object returned by NuGet. It is returned in the results back from the package source but it is not available on the Package object.

Also the download counts are different for the same package if you compare the normal search result with a package version search result. The package version search shows a larger download count number. This may be related to the [stats problem](http://blog.nuget.org/20140603/nuget-stats.html) NuGet had recently. Currently Xamarin Studio is showing the download count it receives. The standard search download counts match those shown in Visual Studio's Manage Packages dialog. For the package version search the download count value matches that shown on the NuGet.org website for an individual package (e.g. https://www.nuget.org/packages/jQuery).

**Fix empty source being selected in Add Packages dialog when package source disabled and All Sources selected**

An empty package source selected in the Add Packages dialog could occur when All Sources was selected in the Add Packages dialog and one of the enabled package sources was unchecked in Preferences. On opening the Add Packages dialog again an empty package source would be displayed as the selected package source.

Now the Add Packages dialog will have the remaining enabled package source selected.

**Fix packages.config marked as deleted by Git when updating packages**

On updating packages in a project, and the project is using Git for version control, then the update was causing Git to show the packages.config file as deleted. This would occur if all the NuGet packages were uninstalled as part of the update which caused NuGet to see that there were no NuGet packages referenced and delete the packages.config file.

Now the packages.config file is shown as modified instead of deleted.

