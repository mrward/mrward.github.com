---
layout: post
title: "NuGet Support in Visual Studio for Mac 7.0"
date: 2017-06-04 12:00
comments: true
categories: NuGet Xamarin VSMac MonoDevelop
---

## New Features

   * .NET Core support
   * NuGet 4.0 support

More information on all the new features and changes in [Visual Studio for Mac 7.0](https://www.visualstudio.com/vs/visual-studio-mac/) can be found in the [release notes](https://www.visualstudio.com/en-us/news/releasenotes/vs2017-mac-relnotes).

## .NET Core Support

.NET Core projects do not show a Packages folder in the Solution window. Instead the NuGet packages are displayed in a NuGet folder, which is inside a Dependencies folder.

{% img /images/blog/NuGetSupportInVisualStudioMac7-0/NetCoreProjectJsonNetPackageInSolutionWindow.png 'Newtonsoft.Json NuGet package in Solution window - .NET Core project' 'Newtonsoft.Json NuGet package in Solution window - .NET Core project' %}

The version of the NuGet package is displayed directly in the Solution window. For other project types you need to right click the package to see the version.

If the NuGet package depends on other packages then these can be seen by clicking on the arrow to expand the dependencies.

{% img /images/blog/NuGetSupportInVisualStudioMac7-0/NetCoreProjectJsonNetPackageExpandedInSolutionWindow.png 'Newtonsoft.Json NuGet package expanded in Solution window - .NET Core project' 'Newtonsoft.Json NuGet package expanded in Solution window - .NET Core project' %}

### Restoring Packages

NuGet packages will be restored automatically on opening a .NET Core project. This can be disabled in preferences by unchecking **Automatically restore packages when opening a solution** in the NuGet - General section.

You can manually restore NuGet packages for .NET Core projects by:

  * Right clicking the Dependencies folder and selecting Restore.
  * Right clicking the NuGet folder and selecting Restore.
  * Selecting Restore NuGet Packages from the Project menu.

Selecting Restore NuGet Packages from the Project menu will restore packages for the project or the solution depending on what is currently selected in the Solution window.

Restoring NuGet packages for a .NET Core project works differently compared with a project that uses a packages.config file. The NuGet packages themselves will be downloaded into the NuGet package cache folder ~/.nuget/packages if they do not exist, as before, but the NuGet packages will not be copied into a packages directory inside the solution's directory. The project file will not contain have any Reference items added when a NuGet package is installed.

When a NuGet restore is run for a .NET Core project three files in the obj directory are created.

  * project.assets.json
  * ProjectName.csproj.nuget.g.props
  * ProjectName.csproj.nuget.g.targets

The project.assets.json file contains the dependencies for your project.

The nuget.g.props and nuget.g.targets files will contain any MSBuild imports that your NuGet package requires and they also define some properties, such as the path to the NuGet package cache on your machine.

These three files are used when building your project to resolve the assemblies to be referenced now that they are no longer explicitly stored in your project file.

### Updating Packages

NuGet packages can be updated by:

  * Right clicking the package inside the NuGet folder and selecting Update.
  * Right clicking the NuGet folder and selecting Update.
  * Right clicking the Dependencies folder and selecting Update.
  * Selecting Update NuGet Packages from the Project menu.

Selecting Update NuGet Packages from the Project menu will update all packages in the project or in the solution depending on what is currently selected in the Solution window.

### Removing Multiple NuGet Packages in One Step

You can remove multiple NuGet packages in one step from a .NET Core project by selecting the packages in the Solution window, right clicking and selecting Remove.

{% img /images/blog/NuGetSupportInVisualStudioMac7-0/NetCoreProjectRemoveMultipleNuGetPackagesInSolutionWindow.png 'Removing multiple NuGet packages in Solution window - .NET Core project' 'Removing multiple NuGet packages in Solution window - .NET Core project' %}

Projects that use a packages.config file do not support removing multiple NuGet packages in one step.

### Installing NuGet Packages

NuGet packages are installed by using the Add Packages dialog in the same way as with other project types. To open the Add Packages dialog for a .NET Core project:

  * Right click the NuGet folder and select Add Packages...
  * Right click the Dependencies folder and select Add Packages...
  * Right click the project and select Add - Add NuGet Packages...
  * From the Project menu select Add NuGet Packages...

When the first NuGet package is installed into a .NET Core project a packages.config file will not be created. Instead the NuGet package will be added as a [PackageReference](https://docs.microsoft.com/en-us/nuget/consume-packages/package-references-in-project-files) that is saved in the project file.

### Package Reference

.NET Core projects do not use a packages.config file to record their NuGet dependencies. Instead the .NET Core project file will contain a [PackageReference](https://docs.microsoft.com/en-us/nuget/consume-packages/package-references-in-project-files) after the NuGet package is installed into the project.

```
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>netcoreapp1.1</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Newtonsoft.Json" Version="10.0.2" />
  </ItemGroup>
</Project>
```

Please note that Visual Studio for Mac currently only supports package references with the new SDK style projects which are used by .NET Core. If you use package references in other project types then the Solution window will not show the packages and a packages.config file will be created if you install a NuGet package.

### Updated Packages Available

For other project types the Solution window will check for updated packages and show this information in the Packages folder. This is not currently supported with .NET Core projects.

## NuGet 4.0 Support

Visual Studio for Mac now includes [NuGet 4.0](https://docs.microsoft.com/en-us/nuget/release-notes/nuget-4.0-rtm).

More information on the new features provided by NuGet 4.- can be found in the [Announcing NuGet 4.0 RTM blog post](http://blog.nuget.org/20170308/Announcing-NuGet-4.0-RTM.html) and the [NuGet 4.0 release notes](https://docs.microsoft.com/en-us/nuget/release-notes/nuget-4.0-rtm).


