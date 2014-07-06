---
layout: post
title: "NuGet Support in Xamarin Studio 5.1"
date: 2014-07-06 10:48
comments: true
categories: NuGet Xamarin MonoDevelop
---

[Xamarin Studio 5.1](http://developer.xamarin.com/releases/studio/xamarin.studio_5.1/xamarin.studio_5.1/) was released last week and it includes some new features for the NuGet addin.

## New Features

   * Searching and installing a specific version of a NuGet package.
   * Support for NuGet package sources defined in a solution specific NuGet.Config file.

More details on all the new features and changes in Xamarin Studio 5.1 can be found in the [release notes](http://developer.xamarin.com/releases/studio/xamarin.studio_5.1/xamarin.studio_5.1/). Now let us take a look at the new NuGet features.

## Searching and installing a specific version of a NuGet package

The Add Packages dialog can now be used to search and install a specific version of a NuGet package. You can search for all versions, a range of versions, or a specific version of a NuGet package. To do this the following search syntax is used:

    PackageId version:VersionNumber

The packages are shown in the Add Packages dialog with the most recent version at the top. At the top right of each package in the package list you will see the package version instead of the download count allowing you to find the particular version you want to install.

{% img /images/blog/NuGetSupportInXamarinStudio5-1/AddPackagesDialogAllAutoMapperPackageVersions.png 'Add Packages dialog - all AutoMapper package versions' 'Add Packages dialog - all AutoMapper package versions' %}

To search for all versions of the AutoMapper NuGet package you can use an asterisk or leave the version number blank.

    AutoMapper version:*

To search for all the 2.1 versions of the AutoMapper NuGet package you can use the search:

    AutoMapper version:2.1
    
This will return a range of versions from 2.1.0 up to but not including 2.2.

{% img /images/blog/NuGetSupportInXamarinStudio5-1/AddPackagesDialogAutoMapperPackageVersions21.png 'Add Packages dialog - AutoMapper 2.1 package versions' 'Add Packages dialog - AutoMapper 2.1 package versions' %}

The package id used in the search must match the id of the NuGet package otherwise no results will be returned.

Searching for package versions is also supported in the universal search at the top right of Xamarin Studio.

{% img /images/blog/NuGetSupportInXamarinStudio5-1/PackageVersionSearchInUniversalSearch.png 'Package version search in universal search' 'Package version search in universal search' %}

## Package sources defined in a solution specific NuGet.Config file

 If package sources are defined in the solution's NuGet.Config file then these will be available in the Add Packages dialog when this solution is opened. The NuGet.Config file can be in the solution directory or in the .nuget subdirectory. An example NuGet.Config file is shown below.
 
    <configuration>
      <packageSources>
        <add key="ASP.NET vNext - MyGet" value="https://www.myget.org/F/aspnetvnext/" />
      </packageSources>
    </configuration>
    
When the solution is opened the package sources defined in the NuGet.config file are available in the drop down list at the top left of the Add Packages dialog.

{% img /images/blog/NuGetSupportInXamarinStudio5-1/AddPackagesWithPackageSourceFromSolutionLevelNuGetConfig.png 'Add Packages dialog - package sources from solution NuGet.Config' 'Add Packages dialog - package sources from solution NuGet.config' %}

## Bug Fixes

**Proxy credentials not being requested for a https package source.**
 
   Added a workaround to NuGet to handle Mono returning a different http response when proxy authentication is required.
   
**Web.config transforms not being applied when installing NuGet packages such as Nancy.Hosting.Aspnet.**
 
   Projects were not being recognised as web projects so transforms were only being applied to app.config files.
   
**XDT remove transforms (Xml Document Transformations) failing on Mono.**

   Modified the XDT library that ships with Xamarin Studio to handle different behaviour on Mono compared with Microsoft's .NET Framework. These transforms are used in the Microsoft.AspNet.Mvc NuGet package.