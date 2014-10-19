---
layout: post
title: "NuGet Support in Xamarin Studio 5.5"
date: 2014-10-19 14:00
comments: true
categories: NuGet Xamarin MonoDevelop
---

## New Features

   * Package version constraints in packages.config files are now supported
   * [Xamarin Components](https://components.xamarin.com/) can now have NuGet package dependencies

More information on all the new features and changes in Xamarin Studio 5.5 can be found in the [release notes](http://developer.xamarin.com/releases/studio/xamarin.studio_5.5/xamarin.studio_5.5/).

## NuGet Package Version Constraints

NuGet allows you to [define a range of package versions that are allowed in your project](http://docs.nuget.org/docs/reference/versioning) using the **allowedVersions** attribute in the packages.config file.

    <packages>
      <package id="Newtonsoft.Json" version="5.0.1" allowedVersions="[5.0,6.0)" targetFramework="MonoAndroid44" />
    </packages>

In the above packages.config file the project has Json.NET 5.0.1 installed and will only allow updates to versions of Json.NET that are below 6.0.
 
When you open the solution in Xamarin Studio, and check for updates is enabled in preferences, you will see updates in the Solution window that are valid given the constraint defined in the packages.config file. In the screenshot below an update is shown for Json.NET 5.0.8 in the Solution window even though Json.NET currently has version 6.0.5 available.

{% img /images/blog/NuGetSupportInXamarinStudio5-5/JsonNet508PackageUpdateAvailableInSolutionWindow.png 'Json.NET 5.0.8 package update available shown in Solution window' 'Json.NET 5.0.8 package update available shown in Solution window' %}

When you update the NuGet packages from the Solution window Xamarin Studio will now update to a NuGet package that meets the version constraints defined in the packages.config. In the Package Console screenshot below the Json.NET package was updated, with the constraint in place, and Json.NET 5.0.8 was installed.

{% img /images/blog/NuGetSupportInXamarinStudio5-5/JsonNet508PackageInstalledInPackageConsole.png 'Json.NET package updated to 5.0.8 - Package Console output' 'Json.NET package updated to 5.0.8 - Package Console output' %}

Note that if you install a NuGet package from the Add Packages dialog you can override the constraint and install a NuGet package with a version outside of the range of the constraint.

## Components with NuGet Packages

A Component from [Xamarin's Component Store](https://components.xamarin.com/) can now declare a dependency on one or more NuGet packages which will be installed into the project when the Component is installed. The [Android Support Library v13 Component](https://components.xamarin.com/view/xamandroidsupportv13-18) is one example that has a NuGet package dependency.

{% img /images/blog/NuGetSupportInXamarinStudio5-5/AndroidSupportLibraryV13ComponentInStore.png 'Android Support Library v13 Component in Component Store' 'Android Support Library v13 Component in Component Store' %}

When you install this Component you will see that it installs the [Xamarin.Android.Support.v13 NuGet package](https://www.nuget.org/packages/Xamarin.Android.Support.v13/).

{% img /images/blog/NuGetSupportInXamarinStudio5-5/AndroidSupportLibraryV13ComponentInstalledInSolutionWindow.png 'Android Support Library v13 Component in Solution window' 'Android Support Library v13 Component in Solution window' %}

In older versions of Xamarin Studio the NuGet package will not be installed and instead the project will reference the Xamarin.Android.Support.v13.dll which is included with the Component.

The NuGet packages a Component depends on are displayed in the **Packages** tab on the Component Details page, which you can open by double clicking the Component in the Solution window, or by right clicking the Component and selecting **Details**.

{% img /images/blog/NuGetSupportInXamarinStudio5-5/AndroidSupportLibraryV13ComponentDetailsWithPackagesTab.png 'Android Support Library v13 Packages in Component Details page' 'Android Support Library v13 Packages in Component Details page' %}

From the **Packages** tab you can also install a NuGet package that a Component depends on if it was removed from the project. So if the Xamarin.Android.Support.v13 NuGet package is removed from the project the Component will be highlighted in red to indicate that there is a problem.

{% img /images/blog/NuGetSupportInXamarinStudio5-5/AndroidSupportLibraryV13ComponentErrorInSolutionWindow.png 'Android Support Library v13 Component error in Solution window' 'Android Support Library v13 Component error in Solution window' %}

If you then open the Component Details page you will see in the **Packages** tab that the NuGet package is missing.

{% img /images/blog/NuGetSupportInXamarinStudio5-5/AndroidSupportLibraryV13ComponentDetailsWithMissingNuGetPackage.png 'Android Support Library v13 Component Details page with missing NuGet Package' 'Android Support Library v13 CComponent Details page with missing NuGet Package' %}

To add the NuGet package back to the project you can hover the mouse over the warning icon and click the Add Package button that appears in the pop-up window.

{% img /images/blog/NuGetSupportInXamarinStudio5-5/AndroidSupportLibraryV13ComponentDetailsWithAddPackagePopUpWindow.png 'Android Support Library v13 Component Details page with Add Package pop-up window' 'Android Support Library v13 CComponent Details page with Add Package pop-up window' %}


