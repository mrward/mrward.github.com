---
layout: post
title: "NuGet Support in Visual Studio for Mac 17.3"
date: 2022-09-01 14:20
comments: true
categories: NuGet Xamarin VSMac MonoDevelop
---

## Changes

   * NuGet 6.2.1 support
   * Improved NuGet restore support for a solution containing classic and SDK style projects
     * Fixed unable to find WorkloadAutoImportPropsLocator SDK restore error with classic and SDK style projects
     * Fixed invalid framework identifer restore error with classic and SDK style projects
     * Fixed error restoring Mapsui.Mac.sln
 
More information on all the new features and changes in [Visual Studio for Mac 17.3](https://www.visualstudio.com/vs/visual-studio-mac/)
can be found in the [release notes](https://docs.microsoft.com/en-us/visualstudio/releases/2022/mac-release-notes).

## NuGet 6.2.1 support
    
[NuGet 6.2.1.2](https://learn.microsoft.com/en-us/nuget/release-notes/nuget-6.2) is now
included with Visual Studio for Mac 17.3.

## Improved NuGet restore support for a solution containing classic and SDK style projects

NuGet restore has been improved for a solution contains a mix of classic 
(non SDK style projects) and SDK style projects. Problems can occur
in this case because classic projects require MSBuild on Mono, whilst SDK style
projects typically require .NET MSBuild.

The following sections contain information about the individual problems that have
been fixed in Visual Studio for Mac 17.3

**Fixed unable to find WorkloadAutoImportPropsLocator SDK restore error with classic and SDK style projects**

If a solution contained a .NET 6 project, and a .NET 4.7.2 SDK style
project, and Building with MSBuild on Mono was unchecked in
Solution Properties - Build - General, the restore
would fail with an error when only .NET 6.0.300 SDK was installed:

    Unable to find SDK 'Microsoft.NET.SDK.WorkloadAutoImportPropsLocator'

To fix this Visual Studio for Mac now does not load referenced
projects when getting restore information for a specific project
using its MSBuild host. When all projects were loaded by the
MSBuild host, the project load would fail since the MSBuildSDKsPath
was not defined because .NET SDK was being used since none
supported using MSBuild on Mono.

**Fixed invalid framework identifer restore error with classic and SDK style projects**

If a solution contained a non-SDK style project then MSBuild on mono was
always used to restore if the solution contained more than 10
projects. This prevented some solutions from being
restored if they contained mix of .NET 6.0 projects and
classic non-SDK style projects, and had a global.json that pinned the .NET SDK
to 6.0.300. The restore would fail when MSBuild on mono was used with an
error about an invalid framework identifier.

```
MonoBundle/MSBuild/Current/bin/NuGet.targets(162,5):
error MSB4018: The "WriteRestoreGraphTask" task failed unexpectedly.
error MSB4018: NuGet.Frameworks.FrameworkException: Invalid framework identifier ''.
error MSB4018:   at NuGet.Frameworks.NuGetFramework.GetShortFolderName (NuGet.Frameworks.IFrameworkNameProvider mappings)
error MSB4018:   at NuGet.Frameworks.NuGetFramework.GetShortFolderName ()
error MSB4018:   at NuGet.ProjectModel.PackageSpecWriter.WriteMetadataTargetFrameworks (NuGet.RuntimeModel.IObjectWriter writer, NuGet.ProjectModel.ProjectRestoreMetadata msbuildMetadata)
error MSB4018:   at NuGet.ProjectModel.PackageSpecWriter.SetMSBuildMetadata (NuGet.RuntimeModel.IObjectWriter writer, NuGet.ProjectModel.PackageSpec packageSpec)
```

MSBuild on mono does not support .NET 6.0.300 sdk and, because of the
global.json file, Visual Studio for Mac did not downgrade to an older supported
.NET SDK which then caused the restore to fail since it cannot resolve the target framework.

To avoid this problem the restore now checks the Build with MSBuild
on Mono setting, in Solution Properties - Build - General, and if unchecked
the restore will succeed.

**Fixed error restoring Mapsui.Mac.sln**

The Mapsui.Mac.sln from https://github.com/Mapsui/Mapsui would fail to
restore with following error when Build with
MSBuild on Mono was disabled in Solution Properties:

    error MSB4019: The imported project "/usr/local/share/dotnet/sdk/6.0.300/Xamarin/iOS/Xamarin.iOS.CSharp.targets" was not found.

The Mapsui.Mac.sln contains .NET 6.0 maui and some classic Xamarin
projects. It is not possible to restore these all projects with either
MSBuild on Mono or .NET's MSBuild. The solution also contains more than
10 projects which results in Visual Studio for Mac running MSBuild directly out of process
instead of using its MSBuild host. Since neither MSBuild on mono, nor .NET's MSBuild can
restore the solution, this would fail.

To fix this Visual Studio for Mac no longer runs MSBuild
directly out of process to run the restore when a solution contains a mix of project types that
require different MSBuild's. Instead Visual Studio for Mac runs the
restore with its MSBuild hosts, which support both MSBuild on Mono and 
.NET's MSBuild. Each project then has the GenerateRestoreGraphFile MSBuild
target run separately using the correct MSBuild. This requires
the Build with MSBuild on Mono option to be unchecked, and allows
the Mapsui.Mac.sln to be restored.