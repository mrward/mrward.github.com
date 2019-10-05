---
layout: post
title: "NuGet Support in Visual Studio for Mac 8.3"
date: 2019-10-06 12:00
comments: true
categories: NuGet Xamarin VSMac MonoDevelop
---

## Changes

   * NuGet 5.3 support
   * Managing NuGet packages for the solution
   * Show NuGet package updates for SDK style projects in the Solution window

More information on all the new features and changes in [Visual Studio for Mac 8.3](https://www.visualstudio.com/vs/visual-studio-mac/)
can be found in the [release notes](https://docs.microsoft.com/en-us/visualstudio/releasenotes/vs2019-mac-relnotes).

## NuGet 5.3 support

[NuGet 5.3.0.6192](https://docs.microsoft.com/en-us/nuget/release-notes/nuget-5.3) is now
included with Visual Studio for Mac 8.3.

## Managing NuGet Packages for the solution

Support for Manage NuGet packages for the solution dialog was originally available in a separate 
[NuGet extensions addin](https://github.com/mrward/monodevelop-nuget-extensions). This support has now
been integrated into the main NuGet addin, with some user interface changes, and is now available by
default in Visual Studio for Mac.

{% img /images/blog/NuGetSupportInVisualStudioMac8-3/ManageNuGetPackagesSolutionBrowseTab.png 'Manage NuGet Packages - Solution - Browse tab' 'Manage NuGet Packages - Solution - Browse tab' %}

The NuGet extensions addin is still available and provides a
NuGet Powershell Console but it no longer includes a Manage NuGet Packages dialog.

The Manage NuGet Packages dialog contains four tabs:

 - Browse
   - Used to search for and install NuGet packages. This is equivalent to the old Add NuGet Packages dialog.
 - Installed
   - Shows the installed NuGet packages.
 - Updates
   - Shows NuGet packages that have new versions available.
 - Consolidate
    - Shows NuGet packages that have multiple versions installed in the solution. Only available when managing
    NuGet packages for the solution.

The Add Packages dialog has been removed and replaced with the Manage NuGet Packages dialog since
you can do everything that was possible with the Add Packages dialog with the new dialog.

The Manage NuGet Packages dialog can be opened to manage the NuGet packages for
the solution in the following ways:

 - Right click the solution in the Solution window and select **Manage NuGet Packages...**
 - Select **Project** - **Manage NuGet Packages...** when the solution is selected in the Solution window 

The Manage NuGet Packages dialog title is different depending on whether the NuGet packages
are being managed for the solution or for the project. When managing packages for the solution
the dialog title will be `Manage NuGet Packages – Solution`.

### Managing NuGet Packages for a single project

To manage NuGet packages for a single project the Manage NuGet Packages dialog can be opened
in the following ways:

 - Right click the project in the Solution window and select Manage NuGet Packages...
   - This was added to make Visual Studio for Mac consistent with Visual Studio on Windows
 - Double click the Packages folder in the Solution window
 - Right click the Packages folder and select Manage NuGet Packages...
 - Double click the Dependencies folder in the Solution window.
     - In previous versions of Visual Studio for Mac this would not open the dialog
 - Right click the Depdendencies folder and select Manage NuGet Packages...
 - Double click the NuGet folder underneath the Dependencies folder
 - Right click the NuGet folder, underneath the Dependencies folder, and select Manage NuGet Packages...
 - Select **Project** - **Manage NuGet Packages...** when the project is selected in the Solution window 

When managing NuGet packages for a single project the dialog title shows the project name 
`Manage NuGet Packages – ProjectName`.

{% img /images/blog/NuGetSupportInVisualStudioMac8-3/ManageNuGetPackagesProject.png 'Manage NuGet Packages for Project' 'Manage NuGet Packages for Project' %}

### Installing NuGet Packages

The Browse tab in the Manage NuGet Packages can be used to search for and install NuGet packages
into one or more projects. This tab is equivalent to the old Add NuGet Packages dialog.

The latest stable NuGet package version is selected by default. This is now indicated by the
version having `(latest stable)` appended in the New Version combo box on the right hand
side of the dialog.

To install a NuGet package to multiple projects:

 - Right click the solution and select Manage NuGet Packages...
 - Search for a NuGet package
 - Click the Add Package button
 - In the Select Projects dialog that is opened, select the projects where you want the NuGet package to be installed,
  and click OK

{% img /images/blog/NuGetSupportInVisualStudioMac8-3/ManageNuGetPackagesSolutionSelectProjects.png 'Manage NuGet Packages - Select Projects dialog' 'Manage NuGet Packages - Select Projects dialog' %}

### Uninstalling NuGet Packages

The Installed tab in the Manage NuGet Packages can be used to uninstall
NuGet packages from one or more projects.

{% img /images/blog/NuGetSupportInVisualStudioMac8-3/ManageNuGetPackagesInstalledTab.png 'Manage NuGet Packages - Installed tab' 'Manage NuGet Packages - Installed tab' %}

To uninstall a NuGet package:

 - Right click the solution and select Manage NuGet Packages...
 - Select the Installed tab
 - Select a NuGet package to uninstall
   - To uninstall multiple NuGet packages use the check boxes in package list
 - Click the Uninstall Packages button
 - In the Select Projects dialog that is opened, select the projects where the NuGet package should be removed, and click OK.

### Updating NuGet Packages

The Updates tab shows the updated NuGet packages available to be installed.

{% img /images/blog/NuGetSupportInVisualStudioMac8-3/ManageNuGetPackagesUpdatesTab.png 'Manage NuGet Packages - Updates tab' 'Manage NuGet Packages - Updates tab' %}

The Updates tab shows the **Current Version** installed. If
multiple versions of the packages are installed, then this will
display 'Multiple' with an information icon where information about the
projects and versions can be viewed in a tooltip. Otherwise the
installed package version is displayed.

{% img /images/blog/NuGetSupportInVisualStudioMac8-3/UpdatesTabMultipleVersions.png 'Manage NuGet Packages - Updates tab - Multiple versions installed' 'Manage NuGet Packages - Updates tab -  Multiple versions installed' %}

{% img /images/blog/NuGetSupportInVisualStudioMac8-3/UpdatesTabMultipleVersionsTooltip.png 'Manage NuGet Packages - Updates tab - Multiple versions installed - tooltip' 'Manage NuGet Packages - Updates tab -  Multiple versions installed - tooltip' %}

To update a NuGet package in multiple projects:

 - Right click the solution and select Manage NuGet Packages...
 - Select the Updates tab
 - Select a NuGet package to update
   - To update multiple NuGet packages use the check boxes in packages list
 - Click the Update Packages button
 - In the Select Projects dialog that is opened, select the projects where the NuGet package should be updated, and click OK.

### Consolidating NuGet Packages

If there are different versions of a NuGet package installed in the solution the
Consolidate tab will show this and provide a way of fixing this.

{% img /images/blog/NuGetSupportInVisualStudioMac8-3/ManageNuGetPackagesConsolidateTab.png 'Manage NuGet Packages - Consolidate tab' 'Manage NuGet Packages - Consolidate tab' %}

When a NuGet package is selected, the right hand side of the dialog shows
all the projects in the solution. The project will checked if it has the selected
package installed. The package version used by the project is also shown.
Projects that have a package to consolidate are shown first in the list
view.

Note that the Consolidate tab is only displayed if NuGet packages are being
managed for the solution.

By default the Consolidate tab will select the latest version available.
This may be different from the latest version installed in the projects.

To Consolidate a NuGet package:

 - Select the NuGet package you want to consolidate.
    - Use the check box next to the NuGet package if you want to consolidate multiple NuGet packages in one step
 - Check or uncheck the projects in the projects list.
    - By default projects that contain the selected NuGet package will be checked
 - Click the Consolidate Package button.

## Show NuGet package updates for SDK style projects in the Solution window

NuGet package updates are now shown in the Solution window for SDK style
projects.

{% img /images/blog/NuGetSupportInVisualStudioMac8-3/SdkStyleProjectPackageUpdates.png 'SDK style project - package updates - solution window' 'SDK style project - package updates - solution window' %}

If an updated package is available this information will now be shown
on the Dependencies folder, the NuGet folder and the top level
package in the Solution window.

Previously NuGet packages updates were only displayed for projects
that used a packages.config file and for non-SDK style projects
that used PackageReferences.

Instead of showning the updated NuGet package version
in the solution window, next to the installed version, an update icon
is displayed with the version information in a tooltip. This prevents
the information taking up a lot of space to the right, which can
happen for long version numbers.

{% img /images/blog/NuGetSupportInVisualStudioMac8-3/SdkStyleProjectPackageUpdateVersionTooltip.png 'SDK style project - package update version tooltip - solution window' 'SDK style project - package update version tooltip - solution window' %}

The Update menu, when right clicking a NuGet package, now shows the
version if check for updates found an update - 'Update to 1.2.3'. Otherwise
the Update menu is displayed.

{% img /images/blog/NuGetSupportInVisualStudioMac8-3/SdkStyleProjectUpdateToMenuItem.png 'SDK style project - update menu item - solution window' 'SDK style project - update menu item - solution window' %}

If there is an update and a warning only the warning icon
will be displayed with the warning message available in the tooltip.
The update information will only be available in the right click
context menu for the NuGet package in this case.

Package versions are now displayed in solution window for non-SDK style projects.

{% img /images/blog/NuGetSupportInVisualStudioMac8-3/NonSdkStyleProjectPackageUpdates.png 'Non-SDK style project - package updates - solution window' 'Non-SDK style project - package updates - solution window' %}

SDK style projects always displayed the package version in the Solution
window but non-SDK style projects did not. To make these consistent the NuGet
package version is now shown in the Solution window for all project types.

Previously for non-SDK style projects the package version was shown
as a menu item when right clicking a NuGet package. This has been removed
since the package version is now displayed next to the package id in the
Solution window.

## Bug Fixes

**Fixed text colour when row selected in Manage NuGet Packages dialog**

When a package was checked in the Add NuGet Packages dialog any
row was selected would display black text instead of white text making it
hard to read. The wrong text colour was being set when a package was
checked. This has been fixed in the Manage NuGet Packages dialog.