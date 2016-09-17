---
layout: post
title: "NuGet Support in Xamarin Studio 6.1"
date: 2016-09-17 12:00
comments: true
categories: NuGet Xamarin MonoDevelop
---

Xamarin Studio 6.1 was released last week as part of the latest stable [Xamarin Platform release](https://blog.xamarin.com/major-updates-ios-10-android-nougat-and-other-tasty-bits/) and it includes changes made to the NuGet support.

## Changes

   * NuGet 3.4.3 support
   * Support for project.json files
   * A specific NuGet package version can now be installed from a list shown in the Add Packages dialog
   * NuGet operations can now be cancelled from the status bar or Package Console
   * Support browsing for a local directory when creating a package source
   * Support forcefully removing a NuGet package when it is missing from all package sources
   * Packages installed in the solution are no longer shown in the Add Packages dialog
   * Only global package sources are now shown in Preferences
   * NuGet version supported is now displayed in the About dialog

More information on all the new features and changes in Xamarin Studio 6.1 can be found in the [release notes](https://developer.xamarin.com/releases/studio/xamarin.studio_6.1/xamarin.studio_6.1/).

## NuGet 3.4.3 support

Xamarin Studio now includes [NuGet 3.4.3](https://docs.nuget.org/release-notes/nuget-3.4.3) which means project.json files are now supported and NuGet packages that only support NuGet 3 or above can now be installed.

## Support for project.json files

The project.json file is a new package file format introduced with NuGet 3 which supports transitive restore. More detailed information on project.json can be found in the [NuGet documentation](https://docs.nuget.org/consume/projectjson-intro).

A project.json file replaces the packages.config file and holds the NuGet packages
being used by the project. One difference you will notice is that the project.json file may not show the same list of NuGet packages that a packages.config file would show. This is because the project.json file only shows the NuGet packages you explicitly install into your project. So if you install say bootstrap you will only see bootstrap in the project.json file even though it depends on jQuery. If you do the same for a packages.config file you would see both bootstrap and jQuery saved in the file. Another difference is that references are not added to your project file (.csproj) when using a
project.json file.

In order to use a project.json file with Xamarin Studio you will need to create the file yourself in the project directory and close and re-open the solution. The project.json file needs to be
available when you open the project otherwise Xamarin Studio will default to using a packages.config file.

An example project.json file for a .NET 4.5 library project is shown below:

    {
      "frameworks": {
        "net45": {}
      }
    }
    
When you add a NuGet package to a project that uses a project.json file the NuGet package 
information will be added into a dependencies section:

     "dependencies": {
       "NUnit": "3.2.1"
     }
     
Please note that when using a project.json file the project will not display a From Packages directory 
inside the References folder. This is because the project file does not have any 
references added to it when using a project.json and the reference information is currently not available from the project system.

Please note that there are [future plans](https://blogs.msdn.microsoft.com/dotnet/2016/05/23/changes-to-project-json/) to move the information stored in a project.json file into the project file.

# NuGet 3 package source

Xamarin Studio now supports using the NuGet 3 package source:

https://api.nuget.org/v3/index.json

This can be added into your package sources in Preferences. It is also the package
source that will be created by default if your global [NuGet.Config file](http://lastexitcode.com/projects/NuGet/FileLocations/) is missing.

## Installing a specific NuGet package version from the Add Packages dialog

Older versions of Xamarin Studio supported being able to install specific package versions by using a package version search in the Add Packages dialog as shown below:

    NUnit version:*

This package version search was not easy to discover and so it has been removed and replaced in Xamarin Studio 6.1 with a combo box that allows a particular version to be selected. The Version combo box is in the bottom right hand corner of the Add Packages dialog as shown in the screenshots below.

{% img /images/blog/NuGetSupportInXamarinStudio6-1/AddPackagesDialog.png 'Add Packages dialog' 'Add Packages dialog' %} 

{% img /images/blog/NuGetSupportInXamarinStudio6-1/AddPackagesDialogWithVersionComboBoxSelected.png 'Add Packages dialog with version combo box selected' 'Add Packages dialog with version combo box selected' %} 

Note that in order to populate the version combo box a second request is sent to the
package source so it may not show all the versions immediately.

Also note that for package sources which are local directories only the latest version will be displayed in the version combo box.

## Cancelling a NuGet operation

With Xamarin Studio you can now cancel the currently running NuGet package operation. This can be done
by clicking the red Stop button in the Status Bar or in the Package Console.

{% img /images/blog/NuGetSupportInXamarinStudio6-1/StatusBarStopButton.png 'Status bar stop button' 'Status bar stop button' %} 

## Adding local package sources

When adding a package source in Preferences it is now easier to create a package source for 
a directory on your local machine. There is now a browse button which will allow you to browse to a directory and add it rather than having to type the full path into the text box.

{% img /images/blog/NuGetSupportInXamarinStudio6-1/AddPackageSourceDialog.png 'Add Package Source dialog' 'Add Package Source dialog' %} 

The Add Package Source dialog has also been changed to make it more obvious that either a URL or a folder can be used as a package source. The URL label has been changed to Location and the placeholder text now specifies that a URL or a folder can be used.

## Forced NuGet package removal

A NuGet package can now be removed when it not restored and is unavailable from all package sources.

With older versions of Xamarin Studio a NuGet package must be restored before it can be 
removed. This is a requirement of NuGet since it requires the original NuGet package to work out what has been installed so it can determine what needs to be uninstalled. NuGet can do more than just update the project file with references and MSBuild .targets files, it may add new files to the project or it may run app.config or web.config transforms.

When the NuGet package removal fails because the NuGet package cannot be restored a dialog will be 
displayed asking whether you want to try to remove the NuGet package anyway. If the OK button is selected 
then Xamarin Studio will:

1. Remove the NuGet package from the packages.config file.
2. Remove any assembly references for the NuGet package from the project file (.csproj).
3. Remove any Imports that refer to .targets or .props files that were included with that NuGet package.

This process may miss files that were added to the project by NuGet but in the majority of cases it should remove the NuGet package successfully without having to manually remove the NuGet package information from the project file.

## Packages installed in solution are no longer shown in Add Packages dialog

With previous versions of Xamarin Studio all packages installed in the solution were shown first in the list of packages in the Add Packages dialog. Packages installed in the solution are now no longer shown in the Add Packages dialog.

## Only global package sources shown in Preferences

The package sources shown in the Preferences dialog are now only read from the global NuGet config file. Per-solution NuGet.Config files located in individual solution directories are no longer read when showing the package sources in Preferences. This is because changes made in Preferences only modifies the global NuGet.Config file.

The package sources shown in the Add packages dialog will still include package sources defined in a solution's NuGet.Config file and is unaffected by this change.

## NuGet version displayed in About dialog

The version of NuGet supported by Xamarin Studio is now displayed in the About dialog when the Show Details button is selected.

{% img /images/blog/NuGetSupportInXamarinStudio6-1/AboutDialog.png 'About dialog' 'About dialog' %} 

## Bug Fixes

**Custom MSBuild  .targets files were not always added to the end of the project**

When installing a NuGet package that has a .targets file the Import element created was grouped with the existing Import elements. This is OK most of the time however if there are other items in the project added after the import then any build targets may fail since these items are included after the import. One example is the netfx-System.StringResources NuGet package which may not find any resource files that occur in the project after its Import element.

Now .targets files are added as the last element in the project file. This also makes the behaviour consistent with how NuGet works in Visual Studio.

**Custom MSBuild .props files were not added to the start of the project**

Installing a NuGet package that included an MSBuild .props file would add an Import element for the .props file at the end of the project file which is incorrect. Now .props files are added to the project file as the first child element inside the Project's root element.

## Known Issues

**Offline package restore**

Package restore may not work when you are offline 
even though the NuGet packages may be available in the local NuGet cache on your machine.

The current workaround is to create a package source that
points to a local directory containing all the required NuGet packages and disable all online
NuGet package sources. With just the local package source enabled you can then restore the NuGet packages when you are 
offline. Note that this problem also affects Visual Studio 2015.