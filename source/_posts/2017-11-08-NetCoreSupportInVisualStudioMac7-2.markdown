---
layout: post
title: ".NET Core Support in Visual Studio for Mac 7.2"
date: 2017-11-08 15:00
comments: true
categories: NETCore Xamarin VSMac MonoDevelop
---

## Changes

   * Support NuGet package fallback folders
   * Support AssetTargetFallback
   * Xamarin.Forms 2.4 support (VS for Mac 7.2.2)
   * Fixed transitive types from references not being available

More information on all the new features and changes in [Visual Studio for Mac 7.2](https://www.visualstudio.com/vs/visual-studio-mac/) can be found in the [release notes](https://www.visualstudio.com/en-us/news/releasenotes/vs2017-mac-relnotes#release-date-october-9-2017---visual-studio-2017-for-mac-720636).

## NuGet package fallback folders support

The .NET Core SDK 2.0 defines a NuGet package fallback folder `/usr/local/share/dotnet/sdk/NuGetFallbackFolder` that can be used when looking
for NuGet packages whilst restoring. This fallback folder is now supported by
Visual Studio for Mac 7.2 so that on restoring a .NET Core 2.0 project the NuGet
packages from the fallback folder will be found and do not need to be downloaded from nuget.org into the 
local machine NuGet package cache `~/.nuget/packages`. This should speed up NuGet package restore for .NET Core 2.0 and .NET Standard 2.0 projects the first time it occurs.

As well as the NuGet fallback folders Visual Studio for Mac will now add the following items to the generated project.assets.json
if they are available:

 - configFilePaths
 - sources
 - warningProperties

## AssetTargetFallback support

The .NET Core 2.0 SDK uses an AssetTargetFallback MSBuild property
defined in an imported SDK MSBuild files. This is used instead
of the PackageTargetFallback property when determining if a NuGet
package is compatible. Currently the AssetTargetFallback property
is set to net461 by the .NET Core 2.0 SDK which allows .NET Core projects to use NuGet
packages that include assemblies that target the full .NET Framework.
The supported fallback frameworks are now added to the generated
project.assets.json file by Visual Studio for Mac when a .NET Core 2.0 project is restored.

## NuGet package restore now fails if package and asset target fallbacks are defined by a project

If both AssetTargetFallback and PackageTargetFallback are defined by a project then the
NuGet restore will fail with an error indicating that they cannot
be used together. This mirrors the behaviour of the .NET Core command
line restore.

## Mark implicit PackageReferences as auto referenced

PackageReference items that have IsImplicitlyDefined set to true
in their metadata now have autoRefererenced set to true in the project.assets.json file.

## Support parsing MSBuild conditions with unquoted properties

The .NET Core 2.0 SDK uses conditions that pass properties to the
Exists function without using single quotes around the MSBuild property.
This is now supported by Visual Studio for Mac.

    <PropertyGroup Condition="Exists($(FileName))">

## Xamarin.Forms 2.4 support

The following sections cover bug fixes made in Visual Studio for Mac 7.2.2
to improve support for [Xamarin.Forms 2.4](https://developer.xamarin.com/releases/xamarin-forms/xamarin-forms-2.4/2.4.0-stable/) and later versions. Xamarin.Forms 2.4 includes .NET Standard support as well as defining default MSBuild items for
.NET Core and .NET Standard projects. These default MSBuild items were not 
handled by Visual Studio for Mac 7.2.0 and earlier versions. The main symptoms of Visual Studio for Mac not supporting Xamarin.Forms 2.4 were:

  - Duplicate .xaml files in the Solution window.
  - Nesting of .xaml and .xaml.cs files not working in Solution window.
  - MSBuild items incorrectly added to SDK style projects.

### Generated NuGet files being imported twice

The generated NuGet files, .nuget.g.targets and .nuget.g.props, 
that are created for .NET Core projects were being imported twice.
Once by Microsoft.Common.props, that is provided with Mono, and once
by Visual Studio for Mac.

This double import was causing a duplicate file 
to be added to the 
project when Xamarin.Forms 2.4 
was used in a .NET Standard project and the .NET Core SDK was not 
installed. This would result in the .xaml file and associated
.xaml.cs file not being nested in the solution window.

### MSBuild items added when new xaml file added to project

Adding a new Xamarin.Forms content page with XAML would incorrectly add an
update item to the project for the .xaml.cs file when an SDK style project
was used.

When a new .xaml file was added to an SDK style project a None item as well as an EmbeddedResource item would incorrectly be added to the project file.

```
<None Remove="MyView.xaml" />

<EmbeddedResource Include="MyView.xaml">
  <Generator>MSBuild:UpdateDesignTimeXaml"</Generator>
</EmbeddedResource>
```

### Compile item added with DependentUpon metadata

Xamarin.Forms 2.4 defines a wildcard update similar to:

    <Compile Update="**\*.xaml.cs" DependentUpon="%(Filename)" />

When a project was saved in Visual Studio for Mac a Compile item was
incorrectly added to the main project with the evaluated value stored in the
DependentUpon element.

### DependentUpon being evaluated incorrectly

Metadata defined for wildcard MSBuild items were being evaluated using
the wildcard item instead of the expanded item. This
was causing .xaml.cs files to not be nested in the Solution window
for Xamarin.Forms 2.4.

Xamarin.Forms 2.4 defines an update item similar to:

    <Compile Update="**\*.xaml.cs" DependentUpon="%(Filename)" />

The DependentUpon property was evaluated using the wildcard item
which resulted in the DependentUpon property being
evaluated as '*.xaml.cs' instead of the filename of the item that was
updated by the wildcard.

### Define MSBuildSDKsPath for MSBuild engine host

MSBuild when run on the command line defines the MSBuildSDKsPath in
its MSBuild.dll.config file. The MSBuild engine host that is used
when building with Visual Studio for Mac now also defines the MSBuildSDKsPath property. Previously this was not being defined.

This fixes a build error when using Xamarin.Forms 2.4.0 in an SDK style
project that targets .NET Standard. Xamarin.Forms 2.4.0 uses default
item imports which were not being included since they are conditionally
imported based on the MSBuildSDKsPath property value:

    <Import Project="$(MSBuildThisFileDirectory)Xamarin.Forms.DefaultItems.props" 
        Condition="'$(MSBuildSDKsPath)'!=''" />

If the SDK style project had a .xaml file and a .xaml.cs file then
the .xaml.g.cs file was not being generated when the project had no files
explicitly defined in the project file. This
then caused a build error about the InitializeComponent method not
being defined.

## Bug Fixes

**Fixed transitive assembly references not available until restart**

Given a solution that contains three .NET Standard projects: LibC
references LibB which references LibA. If the Newtonsoft.Json NuGet
package was installed into LibA the types from this NuGet package
were not available in LibB or LibC until the solution was closed and
re-opened again. Closing and re-opening the solution refreshes the
reference information used by the type system. Now when a NuGet
package is installed into a .NET Core project the projects that
reference this project have their reference information refreshed. Types from
the installed NuGet packages are then available in projects that
reference this updated project either directly or indirectly.

**Fixed transitive project references after editing a project file**

Given a solution that contains three .NET Standard projects: LibC references
LibB which references LibA. If a NuGet package is added to LibA by
editing its project file in the text editor the types from this NuGet
package were not available to LibB or LibC without restarting Visual Studio 
for Mac or until the packages were restored for the solution. Now when the project file
is saved the projects that directly or indirectly reference the project
will be restored.

**Ignore project references with ReferenceOutputAssembly set to false when restoring**

Project references that have ReferenceOutputAssembly are now not
added to the project.assets.json file. This was causing
the NuGet package restore to fail in some cases. For example, if a .NET Standard 
project has a project reference to a .NET Core App project, but has the
ReferenceOutputAssembly set to false, then running dotnet restore from the
command line would work, but the restore would fail in Visual Studio for Mac.

