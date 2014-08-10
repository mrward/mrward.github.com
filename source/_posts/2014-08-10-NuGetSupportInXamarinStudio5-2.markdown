---
layout: post
title: "NuGet Support in Xamarin Studio 5.2"
date: 2014-08-10 12:44
comments: true
categories: NuGet Xamarin MonoDevelop
---

## New Features

   * Automatic package update check
   * Package framework retargeting
   * Support for custom packages directory in NuGet.Config
   * Add all checked packages even if they are not visible

More details on all the new features and changes in Xamarin Studio 5.2 can be found in the [release notes](http://developer.xamarin.com/releases/studio/xamarin.studio_5.2/xamarin.studio_5.2/). Now let us take a more detailed look at the new NuGet features.

## Automatic package update check

On opening a solution Xamarin Studio will check in the background for updated packages used by your projects. When the update check begins the status bar will show a **Checking for package updates** message.

{% img /images/blog/NuGetSupportInXamarinStudio5-2/CheckingForPackageUpdatesStatusBarMessage.png 'Checking for package updates status bar message' 'Checking for package updates status bar message' %}

If Xamarin Studio finds there are new updated packages available then the status bar will show a **Package Updates are available** message.

{% img /images/blog/NuGetSupportInXamarinStudio5-2/PackageUpdatesAreAvailableStatusBarMessage.png 'Package updates are available status bar message' 'Package updates are available status bar message' %}

The **Solution** window will show information about the updates in the Packages folder.

{% img /images/blog/NuGetSupportInXamarinStudio5-2/PackageUpdatesInSolutionWindow.png 'Package updates shown in Solution window' 'Package updates shown in Solution window' %}

The **Packages** folder will show the number of updated packages available for a project. For each package inside the Packages folder you can see the version number for the update.

Note that Xamarin Studio will only show updates that are for non-pre-release packages.

The automatic package update feature can be disabled in **Preferences** by unchecking **Check for package updates when opening a solution**.

{% img /images/blog/NuGetSupportInXamarinStudio5-2/CheckForPackageUpdatesOptionInPreferences.png 'Preferences dialog - Check for package updates when opening a solution' 'Preferences dialog - Check for package updates when opening a solution' %}

## Framework retargeting

A NuGet package will often contain assemblies for several target frameworks. Json.NET, for example, contains assemblies for:

 * .NET 2.0
 * .NET 3.5
 * .NET 4.0
 * .NET 4.5
 * .NET Core 4.5 (Windows Store)
 * Portable Class Library (PCL)
   * .NET 4, Silverlight 5, Windows Phone 8, Windows 8, Windows Phone   Application 8.1
 * Portable Class Library (PCL)
   * .NET 4.5, Windows Phone 8, Windows 8, Windows Phone Application 8.1

When you install this NuGet package into your project the assembly that is referenced is determined by your project's target framework. NuGet will reference the assembly which it considers to be the best match for your project's target framework. So if you install Json.NET into a project that targets .NET 4.5 the Json.NET assembly referenced will be taken from the .NET 4.5 folder inside the NuGet package.

If you change your project's target framework after you have installed the NuGet package your project may be referencing a different assembly compared with what would have been referenced if you had installed it after changing the project's target framework. In some cases the project's target framework may not be compatible with the NuGet package. For example, your project targeted .NET 4.5 and you then changed it to .NET 2.0. In this case you would be referencing a .NET 4.5 assembly that would not work with .NET 2.0. Another example is if you change the PCL profile of your project which could affect which PCL assemblies are used from the NuGet package.

In Xamarin Studio 5.2 if you change your project's target framework then the NuGet packages referenced by your project are checked to see if they are still compatible. The result of this check is displayed in the **Package Console**.

{% img /images/blog/NuGetSupportInXamarinStudio5-2/PackagesNeedRetargetingMessagesInPackageConsole.png 'Packages need retargeting messages in Package Console' 'Packages need retargeting messages in Package Console' %}

In the screenshot above the project's target framework was changed from .NET 4.5 to .NET 2.0 whilst the project had the Json.NET and Moq NuGet packages installed. Json.NET is compatible with .NET 2.0 and can be retargeted. Moq does not support .NET 2.0 and is not compatible.

To retarget an individual NuGet package you can select the package in the **Solution** window, right click and select **Retarget**.

{% img /images/blog/NuGetSupportInXamarinStudio5-2/RetargetSinglePackageSolutionWindowMenuItem.png 'Solution window - Retarget menu item' 'Solution window - Retarget menu item' %}

To retarget all packages in the project you can select **Packages** in the **Solution** window, right click and select **Retarget**.

{% img /images/blog/NuGetSupportInXamarinStudio5-2/RetargetAllProjectPackagesSolutionWindowMenuItem.png 'Solution window - Retarget project packages menu item' 'Solution window - Retarget project packages menu item' %}

Selecting **Retarget** will remove the NuGet package and then add it again so the correct assembly is referenced by your project. The status bar will be updated as the package is retargeted and full details can be seen in the Package Console.

The Retarget menu item is only available if NuGet packages need to be retargeted.

Note that if you retarget a NuGet package that is incompatible with your project's target framework then the retargeting will fail and the NuGet package will be removed from the project.

## Support for custom packages directory in NuGet.Config

When a NuGet package is installed into a project the NuGet packages are by default downloaded into a packages directory inside the solution directory. The location and name of this packages directory can be configured by specifying the repositoryPath in the NuGet.config file.

	<configuration>
	  <config>
	    <add key="repositoryPath" value="../../MyPackages" />
	  </config>
	</configuration>

If you create a NuGet.config file and put it the .nuget directory inside the solution, or in the project's directory, then Xamarin Studio will read the repositoryPath and use it when downloading NuGet packages. The path is relative to the NuGet.config file but you can specify a full path if you need to.

Note that if you make a change to the repositoryPath whilst the solution is open you will need to close and re-open the solution for the changes to be detected.

## Add all checked packages even if they are not visible

The Add Packages dialog will now add all packages that were checked when you click the Add Packages button even if they are not currently being displayed in the dialog. This allows you to run multiple searches in the dialog, check multiple packages and then add them to the project in one step without having to open the Add Packages dialog multiple times. Previously only the checked packages that were displayed in the list of packages would be added to your project. 

## Bug Fixes

**Incorrect path separator used for MSBuild Import**

A NuGet package can contain custom MSBuild .targets and .props files. Previously when a NuGet package was installed on the Mac a forward slash path separator was used when adding the paths for custom MSBuild .targets file. This would cause Xamarin Studio on Windows to fail to compile the project. Now backslashes are used for all paths added to the project file.

Xamarin Studio will also now add a Condition to the project file that checks the imported MSBuild .targets file exists. Without this condition the project cannot be opened in Visual Studio if the NuGet packages are missing.

As an example, if Xamarin.Forms 1.1.1.6206 is installed into a project the following Import element will be added.

  	<Import 
  		Project="packages\Xamarin.Forms.1.1.1.6206\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10\Xamarin.Forms.targets"
  		Condition="Exists('packages\Xamarin.Forms.1.1.1.6206\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10\Xamarin.Forms.targets')" />
   
**Repositories.config not restored**

When NuGet packages were restored for the solution the repositories.config file was not being restored. The repositories.config file contains references to all the projects in the solution that have NuGet packages. This information is used by NuGet to determine whether a NuGet package can be removed from the packages directory when a NuGet package is removed from a project. Without this information a NuGet package that is still referenced by other projects could be removed from the packages directory and cause the compilation to fail.

**Package Dependencies not resolved from enabled package sources**

Xamarin Studio will now use all enabled package sources when resolving dependencies for a NuGet package even if a single package source is selected in the Add Packages dialog. This allows the use of a NuGet package source that has packages that have dependencies that are not available from that particular package source but are available from another package source. 

**Packages are up to date status bar message**

If you have the automatic check for updates disabled and update one or more NuGet packages the status bar will now display a message indicating that the packages are up to date if there are no updates available. Previously Xamarin Studio would show a message that the packages were updated when they were not.

**Updating an unrestored package**

If you have automatic package restore disabled and you attempt to update a NuGet package the package will now be restored before the update is attempted. Previously this would fail when an update was available since NuGet looks at the old package to work out how to remove it from the project before updating to the new NuGet package.