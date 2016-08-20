---
layout: post
title: "NuGet Package Explorer for Xamarin Studio"
date: 2016-08-20 12:00
comments: true
categories: NuGet Xamarin MonoDevelop
---

When diagnosing why a NuGet package cannot be installed into a project the application I use is the excellent [NuGet Package Explorer](https://github.com/NuGetPackageExplorer/NuGetPackageExplorer). Whenever there is an error trying to install a NuGet package, similar to the one shown below, you can open the NuGet package with NuGet Package Explorer and take a look at the target frameworks it supports.

    You are trying to install this package into a project that targets 'MonoAndroid,Version=v6.0',
    but the package does not contain any assembly references or content files that are compatible with that framework.
    
Currently [NuGet Package Explorer](https://github.com/NuGetPackageExplorer/NuGetPackageExplorer) is only available on Windows. On other operating systems you can change the file extension to .zip, extract the contents of the NuGet package, or open it into a zip application, and take a look at the files.

To make exploring NuGet packages easier on operating systems where the NuGet Package Explorer application is not available there is a now [NuGet Explorer addin](https://github.com/mrward/monodevelop-nuget-package-explorer) that you can install into Xamarin Studio or MonoDevelop. With this addin you can open and explore NuGet packages from online package sources or from the local file system.

{% img /images/blog/NuGetPackageExplorerForXamarinStudio/ViewingAndroidSupportNuGetPackageContents.png 'Exploring the Xamarin.Android.Support NuGet package in Xamarin Studio' 'Exploring the Xamarin.Android.Support NuGet package in Xamarin Studio' %}

Now let us take a look in more detail of the features provided by the NuGet Package Explorer addin.

## Features

 * Open and view NuGet packages from online package sources or from the local file system.
 * View the NuGet package metadata.
 * View the NuGet package files.
 * View the NuGet package .nuspec file.
 * Open any file stored inside the NuGet package.
 * Open other NuGet packages that are listed as dependencies.
 
Supports Xamarin Studio 6.0 and MonoDevelop 6.0.

## Opening a NuGet Package from a Package Source

To open a NuGet package from a package source you can select Open NuGet Package from the File menu.

{% img /images/blog/NuGetPackageExplorerForXamarinStudio/FileOpenNuGetPackageMenu.png 'File Open NuGet Package menu' 'File Open NuGet Package menu' %}

Alternatively if you have a project open you can right click the Packages folder in the Solution window and select Open NuGet Package.

{% img /images/blog/NuGetPackageExplorerForXamarinStudio/OpenNuGetPackagePackagesFolderMenu.png 'Open Package menu on Packages folder' 'Open Package menu on Packages folder' %}

This will open the Open Package dialog. This dialog is the same as the Add Packages dialog used when installing a NuGet package and allows you to search for NuGet packages. The dialog is based on the version available with Xamarin Studio 6.1 so it has a version combo box where you can select a specific package version instead of having to remember the syntax for the package version search.

{% img /images/blog/NuGetPackageExplorerForXamarinStudio/OpenPackageDialog.png 'Open NuGet Package dialog' 'Open Package dialog' %}

Select one or more NuGet packages and click the Open Package button to download and display them in Xamarin Studio.

{% img /images/blog/NuGetPackageExplorerForXamarinStudio/ViewingJsonNetPackageContents.png 'Exploring the JSON.NET NuGet package in Xamarin Studio' 'Exploring the JSON.NET NuGet package in Xamarin Studio' %}

The NuGet package metadata is shown on the left. On the right are the files that the NuGet package contains. You can also view the .nuspec file stored in the NuGet package by selecting the NuSpec tab at the bottom of the window.

{% img /images/blog/NuGetPackageExplorerForXamarinStudio/ViewingJsonNetNuSpec.png 'Viewing JSON.NET .nuspec file' 'Viewing JSON.NET .nuspec file' %}

## Exploring Dependencies

NuGet package dependencies are displayed with hyperlinks. 

{% img /images/blog/NuGetPackageExplorerForXamarinStudio/ViewingJsonNetPackageDependencies.png 'Viewing JSON.NET dependencies' 'Viewing JSON.NET dependencies %}

If you click one these hyperlinks the Open Package dialog will be opened and the package will be searched for. You can then choose a package version and open the NuGet package.

## Opening Files Contained Inside the NuGet Package

To open a file contained inside a NuGet package you can double click the file, or right click the file and select Open.

{% img /images/blog/NuGetPackageExplorerForXamarinStudio/OpenFileInsideNuGetPackageMenu.png 'Open menu to open file inside NuGet package into Xamarin Studio' 'Open menu to open file inside NuGet package into Xamarin Studio' %}

The file will then be opened inside Xamarin Studio.

## Opening a NuGet Package File

If you have a NuGet Package (.nupkg) stored on the local machine that is not available from any package source you can open the file directly in Xamarin Studio by selecting Open from the File menu.

You can also associate .nupkg files directly with Xamarin Studio and have them automatically open inside the IDE.

## Opening a NuGet Package Installed in a Project

You can explore NuGet packages that are installed in your project by right clicking the package in the Solution window and selecting Open Package.

{% img /images/blog/NuGetPackageExplorerForXamarinStudio/OpenInstalledPackageMenu.png 'Open installed package context menu' 'Open installed package context menu' %}

## Installing the NuGet Package Explorer addin

The NuGet Package Explorer addin is available from the MonoDevelop addin repository on the beta channel. It can be installed by from the Add-in Manager by searching the gallery and then clicking the Install button.

{% img /images/blog/NuGetPackageExplorerForXamarinStudio/AddinManagerDialog.png 'Addin Manager dialog' 'Addin Manager dialog' %}
