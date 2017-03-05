---
layout: post
title: "NuGet Support in Xamarin Studio 6.2"
date: 2017-03-05 12:00
comments: true
categories: NuGet Xamarin MonoDevelop
---

   * Integrated support for creating NuGet packages with [NuGetizer 3000](https://github.com/nuget/nuget.build.packaging)
   * NuGet 3.5 support
   * Improved project.json support
   * Native License Acceptance dialog

More information on all the new features and changes in Xamarin Studio 6.2 can be found in the [release notes](https://developer.xamarin.com/releases/studio/xamarin.studio_6.2/xamarin.studio_6.2/).

## NuGetizer 3000

[NuGetizer 3000](https://github.com/nuget/nuget.build.packaging) is a set of build tools for creating NuGet packages inspired by [NuProj](http://nuproj.net/). Support for NuGetizer 3000 in Xamarin Studio includes:

 * Project templates for creating NuGet packages
 * Support for creating a NuGet package from an existing project
 * Reference assembly generation for Portable Class Library profiles

The following sections look at the NuGetizer 3000 features provided by Xamarin Studio. For more detailed information on what is supported by NuGetizer 3000 please read the [NuGetizer 3000 feature spec](https://github.com/NuGet/Home/wiki/NuGetizer-3000).

### Project Templates

There are two new project templates available.

 * Multiplatform Library
 * NuGet package

{% img /images/blog/NuGetSupportInXamarinStudio6-2/MultiplatformLibraryNewProjectDialog.png 'Mulitplatform Library project template in New Project dialog' 'Mulitplatform Library project template in New Project dialog' %}

{% img /images/blog/NuGetSupportInXamarinStudio6-2/NuGetPackageNewProjectDialog.png 'NuGet Package project template in New Project dialog' 'NuGet Package project template in New Project dialog' %}

### Multiplatform Library - Single for all platforms

The Mulitplatform Library project template will create a Portable Class Library project with NuGet package metadata when the Single for all platforms option is selected.

{% img /images/blog/NuGetSupportInXamarinStudio6-2/MultiplatformLibraryWizardSingleSelected.png 'Multiplatform library - single for all platforms selected' 'Multiplatform library - single for all platforms selected' %}

To create a NuGet package right click the project and select Create NuGet Package. This will generate a NuGet package (.nupkg) in the output directory with the Portable Class Library assembly in the corresponding portable lib directory inside the NuGet package.

### Multiplatform Library - Platform specific

This Multiplatform Library will create a shared project, an iOS project, an Android project and a NuGet packaging project when the iOS and Android options are selected.

{% img /images/blog/NuGetSupportInXamarinStudio6-2/MultiplatformLibraryWizardPlatformsSelected.png 'Multiplatform library - iOS and Android selected' 'Multiplatform library - iOS and Android selected' %}

The iOS and Android project will reference the shared project. The NuGet packaging project will reference the iOS and Android projects. The NuGet packaging project will contain the NuGet package metadata.

When the NuGet packaging project is built it will create the NuGet package in its output folder and inside the NuGet package will be the Android and iOS output assemblies each in their own platform specific lib folders.

### NuGet Packaging Project

{% img /images/blog/NuGetSupportInXamarinStudio6-2/NuGetPackageWizard.png 'NuGet package project template configuration' 'NuGet package project template configuration' %}

The NuGet packaging project can be used to create a meta NuGet package, which is a NuGet package that has no content itself but references other NuGet packages, or it can reference other projects and it will add their output assemblies to the NuGet package. If the referenced project has NuGet package references then these will be added to the generated NuGet package as dependencies.

NuGet packages can be added to the NuGet packaging project by using the Add Packages dialog.

To include files in the NuGet packaging project the files need to have Include in Package property set to true. This can be done by right clicking the file in the solution window, selecting Properties, then selecting Include in Package from the NuGet section in the Properties window.

{% img /images/blog/NuGetSupportInXamarinStudio6-2/IncludeInPackagePropertiesWindow.png 'Include in Package in Properties window' 'Include in Package in Properties window' %}

### Adding NuGet Package Metadata

NuGet package metadata can be added to any .NET project by selecting the NuGet Package - Metadata page available in the project options.

{% img /images/blog/NuGetSupportInXamarinStudio6-2/NuGetPackageMetadataProjectOptions.png 'NuGet Package metadata in project options' 'NuGet package metadata in project options' %}

The General tab shows compulsory metadata that must be specified for a NuGet package. The Details tab shows other optional metadata that can be specified. This NuGet package metadata is saved directly into the project file (.csproj).

{% img /images/blog/NuGetSupportInXamarinStudio6-2/NuGetPackageMetadataDetailsProjectOptions.png 'NuGet Package metadata in project options' 'NuGet package metadata in project options' %}

Once NuGet package metadata has been added then the NuGet.Build.Packaging NuGet package will be added to the project if it does not already exist. This NuGet package provides MSBuild tasks that are used when creating the NuGet package for the project.

NuGet package metadata should be added to project if you want to be able to create a NuGet package for that project on its own. Without NuGet package metadata the project would need to be referenced by a NuGet packaging project, or another project with NuGet package metadata, for it to be included in a NuGet package.

If a NuGet packaging project references a project that has its own NuGet package metadata then a dependency will be added to the NuGet package created by the NuGet packaging project. The referenced project's output assembly would not be included in the NuGet package created by the packaging project in this case.

### Generating a NuGet Package

To generate a NuGet package you can right click the project and select Create NuGet Package.

{% img /images/blog/NuGetSupportInXamarinStudio6-2/CreateNuGetPackageMenuItem.png 'Create NuGet Package menu item' 'Create NuGet Package menu item' %}

For NuGet packaging projects you can also build the project and it will create a NuGet package.

For a .NET project that has NuGet package metadata you can generate a NuGet package when building the project by enabling the "Create a NuGet Package when building the project" option in the Project Options dialog.

{% img /images/blog/NuGetSupportInXamarinStudio6-2/CreateNuGetPackageWhenBuildingOption.png 'Creating a NuGet Package when building the project option - project options' 'Creating a NuGet Package when building the project - project options' %}

### Adding Platform Implementations

If you have a Portable Class Library with NuGet package metadata you can add platform specific implementations for iOS and Android by right clicking the project and selecting Add - Add Platform Implementation. This will open a dialog where you can choose iOS and Android, and also whether to create a shared project.

{% img /images/blog/NuGetSupportInXamarinStudio6-2/AddPlatformImplementationDialog.png 'Add Platform Implementation dialog' 'Add Platform Implementation dialog' %}

After clicking OK an Android and iOS project will be created along with a NuGet packaging project that references these projects. The NuGet package metadata will be moved from the Portable Class Library project to the NuGet packaging project. If the shared project option was selected then the code from the Portable Class Library project will be moved to the shared project. The shared project will be referenced by the iOS and Android project.

### Reference Assembly Generation

In the Project Options for a NuGet Packaging project there is a Reference Assemblies page in the NuGet Package category. This pages shows a list of Portable Class Library profiles that can be selected.

{% img /images/blog/NuGetSupportInXamarinStudio6-2/ReferenceAssembliesProjectOptions.png 'Reference assemblies in project options' 'Reference assemblies in project options' %}

Reference assemblies for the selected profiles will be generated based on the output assemblies from the projects referenced by the NuGet packaging project.

That brings us to the end of the walkthrough of the NuGetizer 3000 features provided by Xamarin Studio. 

## NuGet 3.5 Support

Xamarin Studio now includes [NuGet 3.5](https://docs.microsoft.com/en-us/nuget/release-notes/nuget-3.5-rtm).

More information on the new features provided by NuGet 3.5 can be found in the [Announcing NuGet 3.5 RTM blog post](http://blog.nuget.org/20161027/Announcing-NuGet-3.5-RTM.html) and the [NuGet 3.5 release notes](http://docs.nuget.org/ndocs/release-notes/nuget-3.5-RTM).

## Improved project.json Support

NuGet packages are now automatically restored when the project.json file is saved in the text editor.

## Native License Acceptance Dialog

The Licence Acceptance dialog now uses native UI.

{% img /images/blog/NuGetSupportInXamarinStudio6-2/LicenseAcceptanceDialog.png 'License Acceptance dialog' 'License Acceptance dialog' %}

## Bug Fixes

**Updating all NuGet packages installs unexpected pre-release NuGet packages**

With a pre-release NuGet package installed in a project, updating
all the NuGet packages in the project could cause the stable NuGet
packages to be updated to pre-release versions. On updating all the packages if any were pre-release the include
pre-release flag would be set on updating which would allow any
pre-release NuGet packages to be used on updating. Now the include pre-release flag is not set on updating. NuGet 3
will still update pre-release NuGet packages to the latest pre-release
or latest stable, depending on which is the latest version, without
this include pre-release flag being set. NuGet 2 required the include
pre-release flag set to update a pre-release to the latest pre-release.

An example - project has the following NuGet packages
installed:

    Xamarin.Forms 2.3.3.180
    Xamarin.Forms.CarouselView 2.3.0-pre1

Latest available packages from nuget.org:

    Xamarin.Forms 2.3.3.180 (latest stable)
    Xamarin.Forms 2.3.4.184-pre1 (latest pre-release)
    Xamarin.Forms.CarouselView 2.3.0-pre2

Expected result on updating all packages in the project:

    Xamarin.Forms 2.3.3.180
    Xamarin.Forms.CarouselView 2.3.0-pre2

Actual result:

    Xamarin.Forms 2.3.4.184-pre1
    Xamarin.Forms.CarouselView 2.3.0-pre2

**Types unavailable when NuGet package added to project.json file**

When a NuGet package was added to a project that used a project.json file 
the types from the assembly provided by the NuGet package were not
available until the project was reloaded or the references were
modified. This prevented code completion from showing the correct information.

