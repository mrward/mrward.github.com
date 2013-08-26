---
layout: page
title: "MonoDevelop NuGet Addin 0.6"
date: 2013-08-26 12:58
comments: true
sharing: true
footer: true
---

## New Features

* [NuGet 2.7](http://docs.nuget.org/docs/release-notes/nuget-2.7) support.
* Support for restoring NuGet packages.
* Added message to show that the Manage Packages dialog is searching for packages.

### Restoring NuGet Packages

To restore NuGet packages for your solution, from the **Solution** window right click the solution, project or References, and select **Restore NuGet Packages**.

![Restore NuGet Packages menu item](./RestoreNuGetPackagesMenuItem.png)

This will run NuGet.exe and uses the new **restore** argument that has been added to NuGet 2.7. The full command line will be similar to the following:

    mono --runtime=v4.0 NuGet.exe restore YourSolution.sln

The output from this command will be shown in the NuGet output window. In the screenshot below you can see jQuery, Json.NET and NUnit being restored.

![NuGet Restore Output Window](./NuGetPackageRestoreOutputWindow.png)

This feature assumes that your path environment variable includes the path to the mono runtime so the mono command line can be run without having to specify the full path.

## Bug Fixes

Fixed rootnamespace not being replaced in [source code transforms](http://docs.nuget.org/docs/creating-packages/configuration-file-and-source-code-transformations) (.pp).
 
## Download

The source code for the addin is available on [GitHub](https://github.com/mrward/monodevelop-nuget-addin).

The addin is also available to download from a custom MonoDevelop repository:

* [Xamarin Studio 4.1](http://mrward.github.com/monodevelop-nuget-addin-repository/4.1/main.mrep)
* [Xamarin Studio 4.0](http://mrward.github.com/monodevelop-nuget-addin-repository/4.0/main.mrep)
* [MonoDevelop 3.0](http://mrward.github.com/monodevelop-nuget-addin-repository/3.0.5/main.mrep)


