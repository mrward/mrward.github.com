---
layout: post
title: "NuGet Support in Visual Studio for Mac 8.7"
date: 2020-08-15 12:40
comments: true
categories: NuGet Xamarin VSMac MonoDevelop
---

## Changes

   * NuGet 5.7 support
   * .NET Core 3.1.401 SDK support
     * Supported in Visual Studio for Mac 8.7.1
   * .NET Core 5 preview 6 and 7 support
   * Fixed tests not discovered by VS Test adapter for imported NuGet packages
   * Fixed dependent projects not being restored after project reference added
   * Fixed SDK resolution errors with vstool
 
More information on all the new features and changes in [Visual Studio for Mac 8.7](https://www.visualstudio.com/vs/visual-studio-mac/)
can be found in the [release notes](https://docs.microsoft.com/en-us/visualstudio/releasenotes/vs2019-mac-relnotes).

## NuGet 5.7 support
    
[NuGet 5.7.0.6702](https://github.com/NuGet/docs.microsoft.com-nuget/blob/db494772479ae44c2c00d10c9b01b0136bcee892/docs/release-notes/NuGet-5.7.md) is now
included with Visual Studio for Mac 8.7.

## .NET Core 3.1.401 SDK support

The NuGet packages shown in the Solution window in the Dependencies folder
would not show child dependencies when .NET Core 3.1.401 SDK
was installed.

The ResolvePackageDependenciesDesignTime MSBuild target's behaviour changed
in .NET Core 3.1.401 to no longer return the full set
of NuGet dependencies and now returns only the top level depdnencies.

The logic that was originally in this design time MSBuild target has been
added to Visual Studio for Mac so the full tree structure of the NuGet
package dependencies can be displayed in the Solution window.

Note that .NET Core 3.1.401 is supported in Visual Studio for Mac 8.7.1

## .NET Core 5 preview 6 and 7 support

Restoring an ASP.Core project targeting .NET 5 would fail with an error
when the ResolvePackageAssets task was run.

```
/usr/local/share/dotnet/sdk/5.0.100-preview.6.20304.10/Sdks/Microsoft.NET.Sdk/targets/Microsoft.PackageDependencyResolution.targets(5,5):
Error MSB4018: The "ResolvePackageAssets" task failed unexpectedly.
System.MissingFieldException: Field not found: string NuGet.ProjectModel.LockFileItem.AliasesProperty Due to: Could not find field in class
  at Microsoft.NET.Build.Tasks.ResolvePackageAssets+CacheWriter.WriteItems[T] (NuGet.ProjectModel.LockFileTarget target, System.Func`2[T,TResult] getAssets, System.Func`2[T,TResult] filter, System.Action`2[T1,T2] writeMetadata) [0x0008e] in <9d95d6a4081d41288f54edb04b336893>:0 
```

NuGet 5.7 introduced a new AliasesProperty to the LockFileItem class. MSBuild in Mono 6.12 uses NuGet 5.6 build tasks which
do not support this new property causing the NuGet restore to fail.

Visual Studio for Mac now includes version 5.7 of the NuGet.Build.Tasks assembly and associated
MSBuild targets. These files are copied to Visual Studio for Mac's MSBuild host
when it is created and replace these files originally copied from Mono's MSBuild.

Note that .NET Core 5 preview 8 is not supported by Visual Studio for Mac 8.7 but is supported in by
Visual Studio for Mac 8.8.

## Bug Fixes

**Fixed tests not discovered by VS Test adapter for imported NuGet packages**

Test adapter NuGet PackageReferences defined in a Directory.Build.props
file were not considered by Visual Studio for Mac.
This resulted in the Unit Tests window not showing any unit tests.
Only when the test adapter PackageReferences were in the main project file
would tests be discovered.

**Fixed dependent projects not being restored after project reference added**

For a new Blazor Web Assembly project (with ASP.NET Core hosted)
adding a new Razor library project, and having the Client project
reference the library project, would result in build warnings
incorrectly displayed for the
Server project due to conflicts between the NuGet packages used by
the Client because of the new Razor library project reference.

    MSB3277: Found conflicts between different versions of "Microsoft.AspNetCore.Components" 

Running a restore for the solution would fix this build warning.

With two SDK style projects referencing each other as follows:

ProjectB -> ProjectA

If a new SDK project was added, and ProjectA referenced it, the
restore would only occur for ProjectA not ProjectB. This would mean
the Lib project was not available to ProjectB until a restore was
run for ProjectB or for the solution. Depending on the NuGet packages used
this could result in build warnings.

To fix this all the projects, that reference the project which is modified
due to the new project reference, are now restored.

**Fixed SDK resolution errors with vstool**

vstool is an application that is included with Visual Studio for Mac and provides
a way to run some IDE features from the command line. When one of these features
used a project that required an SDK provided by NuGet it would fail to resolve and download that SDK.

For example, running the following would fail to resolve SDKs provided by NuGet packages. 

    vstool gettext-update --sort -f:Main.sln

An error would be reported:

    ERROR: Unable to find SDK 'Xamarin.Mac.Sdk'

The NuGet SDK resolver would fail to find the NuGet.Credentials.dll
causing vstool to fail.

    FileNotFoundException Could not load file or assembly 'NuGet.Credentials, Version=5.6.0.5.

To fix this the NuGet.Credentials.dll is pre-loaded by Visual Studio for Mac so it is
available in the AppDomain when vstool is run.
