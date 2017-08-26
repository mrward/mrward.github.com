---
layout: post
title: "NuGet Support in Visual Studio for Mac 7.1"
date: 2017-08-26 14:00
comments: true
categories: NuGet Xamarin VSMac MonoDevelop
---

## New Features

   * NuGet 4.3 support
   * Support PackageReference in non .NET Core projects
   * Enable file template when a NuGet package is installed

More information on all the new features and changes in [Visual Studio for Mac 7.1](https://www.visualstudio.com/vs/visual-studio-mac/) can be found in the [release notes](https://www.visualstudio.com/en-us/news/releasenotes/vs2017-mac-relnotes#release-date-august-14-2017---visual-studio-for-mac-7101297).

## NuGet 4.3 Support

NuGet 4.3.0.2418 is now included with Visual Studio for Mac 7.1.

This version of NuGet adds support for .NET Core 2.0 and .NET Standard 2.0
target frameworks.

## Support PackageReference in non .NET Core projects

Non .NET Core Sdk style projects that use a PackageReference MSBuild item are now supported.

    <PackageReference Include="Newtonsoft.Json">
      <Version>10.0.1</Version>
    </PackageReference>

In Visual Studio for Mac 7.0 the Solution
window would not show any packages for the project, would not allow
the packages to be restored, and would create a packages.config file when installing
a NuGet package even though the project was using PackageReferences.

Now if the project has a PackageReference the Packages folder
shows the installed packages and can be used to update or install
NuGet packages which will also create PackageReference MSBuild items.

If the project has no PackageReferences then by default a
packages.config file will be created when a NuGet package is installed.

Currently it is not possible to opt-in to using PackageReferences by default.
So the project file will need to be edited in the text editor to include at
least one PackageReference before the default behaviour of using a packages.config
file is overridden.

## Enable file template when a NuGet package is installed

A file template can now specify that it should be enabled if the project
has a specific reference or has a specific NuGet package installed.
Previously it was only possible to enable a file template based on the
references defined in the project file.

    <HasPackageOrReference PackageId="Xamarin.Forms" Assembly="Xamarin.Forms" />

Whilst a reference will work for projects that use a packages.config
file, if the project uses a project.json file or PackageReferences
then checking the references defined in the project would not find any matches and
the file template would not be enabled.

## Bug Fixes

**Do not require description when creating a new NuGet package project**

When creating a NuGet package project or a multiplatform project the description
needed to be specified in the New Project dialog.

To simplify the project creation
process the package id is used as the description by default. As you type the
package id into the New Project dialog the description text
box will be updated. The description can be changed to be different
to the package id if required.

**Fix being unable to load NuGet Package Project created by Visual Studio on Windows**

Visual Studio on Windows creates a NuGet package project (.nuproj)
with no target framework version which resulted in 4.0 being used by
default in Visual Studio for Mac. This would cause the project to fail to
load in Visual Studio for Mac since it was checking for 4.5 or later.

**Fix no packages shown in Packages folder for NuGet Package Projects**

Opening a previously created NuGet package project (.nuproj) would show no
packages in the Packages folder.

The problem was that the PackageReference project item was defined by
both the NuGet addin and the Packaging addin. The NuGet addin's PackageReference
project item was used instead of the one defined by the Packaging addin so no
package references were found for the NuGet package project.

**Fix NuGet Package Project's MSBuild targets not being created**

If the NuGet package project's generated .nuget.targets or .nuget.props are missing
then these are now created on opening the solution if
automatic restore is enabled.

**Fix being unable to package .NET Standard projects**

A NuGet package project that referenced an Sdk style .NET Standard project
would fail to build the NuGet package. The build would fail with an error:

    Error: Project targets '.NETStandard,Version=v1.4'. It cannot be
    referenced by a project that targets 'NuGet,Version=v1.0'

Updating to a more recent NuGet.Build.Packaging NuGet
package, such as version 0.1.276, fixes this problem.

**NuGet Package project treated as .NET Core project**

NuGet package projects (.nuproj) that use the NuGet.Build.Packaging
0.1.276 NuGet package define a TargetFramework property in an
imported MSBuild .props file. This was causing the project to
be treated as a .NET Core project by Visual Studio for Mac. This caused
the Dependencies folder to be displayed and the References folder
to be removed. Visual Studio for Mac now checks the project has the
Sdk attribute instead of looking at the MSBuild properties defined
by the project when determining the project type.
