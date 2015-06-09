---
layout: post
title: "Paket Support in Xamarin Studio"
date: 2015-06-09 19:00
comments: true
categories: Paket Xamarin MonoDevelop
---

Xamarin Studio and MonoDevelop now have support for [Paket](http://fsprojects.github.io/Paket/) with an alpha release of the [Paket Addin](https://github.com/mrward/monodevelop-paket-addin).

Paket is a dependency manager for .NET. The dependencies it supports are [NuGet packages](http://fsprojects.github.io/Paket/nuget-dependencies.html), files from [GitHub, Gists](http://fsprojects.github.io/Paket/github-dependencies.html) or files from any [HTTP source](http://fsprojects.github.io/Paket/http-dependencies.html). Paket can be used to maintain project dependencies completely from the command line.

So let us take a look at the support for Paket in Xamarin Studio and MonoDevelop.

## Features

   * View dependencies and referenced NuGet packages in the Solution window.
   * Add, remove, update NuGet packages from the Solution window.
   * Install, restore, simplify NuGet packages from the Solution window.
   * Check for updated NuGet packages from the Solution window.
   * Syntax highlighting for all paket files.
   * Code completion whilst editing the paket.dependencies file.
   * Integrates with Xamarin Studio's unified search.
   * paket.dependencies and paket.template file templates.

## Installing the addin

The addin is currently available from [MonoDevelop's Add-in Repository](http://addins.monodevelop.com/) in the alpha channel. In Xamarin Studio open the Add-in Manager and select the Gallery tab. Click the repository drop down and if **Xamarin Studio Add-in Repository (Alpha Channel)** is not displayed then click **Manage Repositories...**. In the window that opens tick the check box next to **Xamarin Studio Add-in Repository (Alpha Channel)** and then click the Close button.

{% img /images/blog/PaketSupportInXamarinStudio/AddingAlphaChannelAddins.png 'Enabling alpha channel addins' 'Enabling alpha channel addins' %}

Back in the Add-in Manager dialog click the Refresh button to update the list of addins. Use the search text box in the top right hand corner of the dialog to search for the addin by typing in **Paket**.

{% img /images/blog/PaketSupportInXamarinStudio/AddinManagerPaketAddin.png 'Paket addin selected in Addin Manager dialog' 'Paket addin selected in Addin Manager dialog' %}

Select the Paket addin and then click the **Install...** button.

Now let us take a look at adding a NuGet package to your project with Paket. This is a simple way to get started with Paket in Xamarin Studio without having to manually create any paket files.

## Adding a NuGet Package

To add a NuGet package using Paket, right click the project in the Solution window, and select Add - Add NuGet Packages using Paket.

{% img /images/blog/PaketSupportInXamarinStudio/AddNuGetPackagesUsingPaketSolutionWindowMenu.png 'Add NuGet Package using Paket Solution window context menu' 'Add NuGet Package using Paket Solution window context menu' %}

The Add NuGet Packages using Paket menu is also available from the main Project menu.

This opens the Add NuGet Packages dialog. Search for the NuGet package you want to use and click the Add Package button.

{% img /images/blog/PaketSupportInXamarinStudio/AddNuGetPackagesDialog.png 'Add NuGet Packages dialog' 'Add NuGet Packages dialog' %}

The Status Bar will update as the NuGet package is installed.

{% img /images/blog/PaketSupportInXamarinStudio/JsonNetPackageAddedStatusBarMessage.png 'Json.NET added status bar message' 'Json.NET added status bar message' %}

More detailed information about the installation can be found in the Paket Console window. This can be opened by clicking the Status Bar or from the View - Pads menu.

{% img /images/blog/PaketSupportInXamarinStudio/JsonNetPackageAddedPaketConsole.png 'Json.NET added Paket Console messages' 'Json.NET added Paket Console messages' %}

After the NuGet package has been installed successfully you will see two new items in the Solution window. A Paket Dependencies folder and a Paket References folder.

{% img /images/blog/PaketSupportInXamarinStudio/PaketFoldersInSolutionWindow.png 'Paket folders in Solution window' 'Paket folders in Solution window' %}

These folders show the NuGet packages that are in the paket.dependencies and paket.references files.

## Paket Dependencies Folder

The Paket Dependencies folder is shown in the Solution window if Xamarin Studio finds a paket.dependencies file in the same directory as the solution. The NuGet packages that are in the paket.dependencies file are shown under this folder.

{% img /images/blog/PaketSupportInXamarinStudio/PaketDependenciesFolderInSolutionWindow.png 'Paket Dependencies folder in Solution window' 'Paket Dependencies folder in Solution window' %}

Double clicking the folder will open the paket.dependencies file into the text editor. The Paket Dependencies folder also has a context menu where you can run Paket commands.

{% img /images/blog/PaketSupportInXamarinStudio/PaketDependenciesFolderContextMenu.png 'Paket Dependencies folder context menu' 'Paket Dependencies folder context menu' %}

From the context menu you can Add a NuGet Package as a dependency, install, restore, update, and simplify your dependencies, or check for updates. When you select Check for Updates the updated NuGet package information will be shown in the Paket Console and in the Solution window.

{% img /images/blog/PaketSupportInXamarinStudio/PaketDependenciesFolderNuGetPackageUpdateInformation.png 'Paket Dependencies folder NuGet package update information' 'Paket Dependencies folder NuGet package update information' %}

To update a single NuGet package you can right click it and select Update. To remove the NuGet package as a dependency you can right click it and select Remove or press delete.

{% img /images/blog/PaketSupportInXamarinStudio/PaketDependencyNuGetPackageContextMenu.png 'Paket Dependencies folder NuGet package context menu' 'Paket Dependencies folder NuGet package context menu' %}

## Paket References Folder

The Paket References folder is shown in the Solution window if Xamarin Studio finds a paket.references file in the same directory as the project. The NuGet packages that are in the paket.references file are shown under this folder. Double clicking the folder will open the paket.references file into the text editor.  

{% img /images/blog/PaketSupportInXamarinStudio/PaketReferencesFolderInSolutionWindow.png 'Paket References folder in Solution window' 'Paket References folder in Solution window' %}

Right clicking the Paket References folder allows you to add a NuGet package to the project.

{% img /images/blog/PaketSupportInXamarinStudio/PaketReferencesFolderAddPackagesMenu.png 'Paket References folder context menu' 'Paket References folder context menu' %}

A NuGet package can be removed by right clicking it and selecting Remove or by pressing Delete.

{% img /images/blog/PaketSupportInXamarinStudio/PaketReferencesFolderNuGetPackageContextMenu.png 'Paket References folder NuGet package context menu' 'Paket References folder NuGet package context menu' %}

## Code Completion

When editing the paket.dependencies file you will get code completion as you type. You can also bring up the code completion list by pressing Ctrl+Enter.

{% img /images/blog/PaketSupportInXamarinStudio/PaketDependenciesFileKeywordCompletion.png 'paket.dependencies file keyword completion' 'paket.dependencies file keyword completion' %}

Keywords that have an associated value will also show code completion after a space is pressed or the first character is typed in.

{% img /images/blog/PaketSupportInXamarinStudio/PaketDependenciesFileKeywordValueCompletion.png 'paket.dependencies file keyword value completion' 'paket.dependencies file keyword value completion' %}

After the source keyword you will see a list of NuGet package sources that are defined in your NuGet.Config file.

{% img /images/blog/PaketSupportInXamarinStudio/PaketDependenciesFileNuGetSourceCompletion.png 'paket.dependencies file NuGet source completion' 'paket.dependencies file NuGet source completion' %}

After the nuget keyword you will see a list of NuGet packages. 

{% img /images/blog/PaketSupportInXamarinStudio/PaketDependenciesFileNuGetPackageCompletion.png 'paket.dependencies file NuGet package completion' 'paket.dependencies file NuGet package completion' %}

This list of NuGet packages is currently taken from your local machine's NuGet cache. Currently there is no support for asynchronously searching an online NuGet package source to get the list of NuGet packages.

## Running Paket commands

Paket commands can be run from the Unified search. If you type in paket you will see some of the Paket commands.

{% img /images/blog/PaketSupportInXamarinStudio/PaketCommandsInUnifiedSearch.png 'Paket commands in unified search' 'Paket commands in unified search' %}

The syntax for each command is the similar to what the paket.exe console application supports but the commands do not support all the parameters.

As you type more of the command the list of commands will be filtered. To run a command select it and then press the enter key. These commands directly run paket.exe and update the paket files and project files. The status of the current command is shown in the Status Bar and the output from paket.exe is shown in the Paket Console window.

{% img /images/blog/PaketSupportInXamarinStudio/PaketDependenciesInstalledStatusBarMessage.png 'Paket dependencies installed status bar message' 'Paket dependencies installed status bar message' %}

{% img /images/blog/PaketSupportInXamarinStudio/PaketDependenciesInstalledPaketConsoleOutput.png 'Paket dependencies installed console message' 'Paket dependencies installed console message' %}

The Paket Console window will automatically be displayed if there was an error running a command and an error message will be displayed in the Status Bar.

{% img /images/blog/PaketSupportInXamarinStudio/StatusBarAddNuGetPackagedErrorMessage.png 'Paket error message in Status Bar' 'Paket error message in Status Bar' %}

{% img /images/blog/PaketSupportInXamarinStudio/PaketConsoleAddNuGetPackagedErrorMessage.png 'Paket console error message' 'Paket console error message' %}

Otherwise you can open the Paket Console by clicking the Status Bar.

## Syntax highlighting

Syntax highlighting is available for all paket files - paket.dependencies, paket.references, paket.lock and paket.template.

{% img /images/blog/PaketSupportInXamarinStudio/PaketDependenciesFileSyntaxHighlighting.png 'paket.dependencies file syntax highlighting' 'paket.dependencies file syntax highlighting' %}

{% img /images/blog/PaketSupportInXamarinStudio/PaketLockFileSyntaxHighlighting.png 'paket.lock file syntax highlighting' 'paket.lock file syntax highlighting' %}



This brings us to the end of the introduction to Paket support in Xamarin Studio.
