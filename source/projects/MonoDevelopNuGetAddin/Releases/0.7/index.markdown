---
layout: page
title: "MonoDevelop NuGet Addin 0.7"
date: 2013-11-03 15:20
comments: true
sharing: true
footer: true
---

## New Features

* [NuGet 2.7.1](http://docs.nuget.org/docs/release-notes/nuget-2.7.1) support.
* Improved support for Portable Class Libraries (PCLs) on Linux and Mac.
* Support using package restore with different parallel mono environments.

## Bug Fixes

Fixed updated pre-release packages not being shown in the Updated tab of the Manage Packages dialog.

## Thanks

* [Kevin John-Philip](https://github.com/kjohnphillip) - adding support for using package restore with different parallel mono environments.
* [Urs Keller](https://github.com/takoyakich) - investigation and initial implementation of PCL support on Mac.

### Improved PCL Support

PCL support has been improved with this version of the addin. NuGet has been customised so it can now find the PCL profiles on both Linux and Mac. NuGet has also been modified to support installing a portable NuGet package into a portable .NET project when one or both of these support Mono frameworks.
 
Finding the PCL profiles on Linux and Mac is based on how [xbuild locates the portable reference assemblies](https://github.com/mono/mono/blob/master/mcs/class/Microsoft.Build.Tasks/Microsoft.Build.Tasks/GetReferenceAssemblyPaths.cs) by checking certain directories. The following directories are checked for PCL profiles:

1. Paths defined in the environment variable $XBUILD_FRAMEWORK_FOLDERS_PATH with the .NETPortable directory appended.
2. /Library/Frameworks/Mono.framework/External/xbuild-frameworks/.NETPortable on Mac only
3. $prefix/lib/mono/xbuild-frameworks/.NETPortable

NuGet will use first directory that exists based on the above directories.

NuGet has also been changed to allow the installation of a portable NuGet package, such as Json.NET, which does not explicitly support Mono frameworks, into a portable .NET project. To do this the Mono supported frameworks of a PCL are treated as optional. The mono frameworks will be used when checking compatibility only if both the NuGet package and the local machine have Mono supported frameworks.

There is currently no support for installing a portable NuGet package into a project that targets a Mono framework such as MonoAndroid. This is due to how NuGet embeds the supported frameworks into the NuGet package. If you want to do this the best approach will be to create a portable .NET project and add the NuGet package to that project. Then you can reference the portable .NET project.

Installing a portable NuGet package into a project that targets .NET 4.0 or .NET 4.5 worked in previous versions of NuGet and is still supported.

[NuGet 2.7.2 should have a fix for portable packages being installed when the corresponding profile has had additional platforms added](https://nuget.codeplex.com/workitem/2926) so hopefully this can be used instead of the workaround added to the version of NuGet that is included with the addin.

## Download

The source code for the addin is available on [GitHub](https://github.com/mrward/monodevelop-nuget-addin).

The addin is also available to download from a custom MonoDevelop repository:

* [Xamarin Studio 4.1](http://mrward.github.com/monodevelop-nuget-addin-repository/4.1/main.mrep)
* [Xamarin Studio 4.0](http://mrward.github.com/monodevelop-nuget-addin-repository/4.0/main.mrep)
* [MonoDevelop 3.0](http://mrward.github.com/monodevelop-nuget-addin-repository/3.0.5/main.mrep)


