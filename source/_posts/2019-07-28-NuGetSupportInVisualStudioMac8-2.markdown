---
layout: post
title: "NuGet Support in Visual Studio for Mac 8.2"
date: 2019-07-28 12:00
comments: true
categories: NuGet Xamarin VSMac MonoDevelop
---

## Changes

   * NuGet 5.1 support
   * Support partial installs into multi-target framework projects
   * Fixed no network error displayed in Add Packages dialog
   * Fixed restoring too many projects when target framework changed
   * Fixed adding non-sdk style project triggering NuGet restore multiple times
   * Fixed restore menu incorrectly disabled

More information on all the new features and changes in [Visual Studio for Mac 8.2](https://www.visualstudio.com/vs/visual-studio-mac/)
can be found in the [release notes](https://docs.microsoft.com/en-us/visualstudio/releasenotes/vs2019-mac-relnotes).

## NuGet 5.1 support

[NuGet 5.1.0.6013](https://docs.microsoft.com/en-us/nuget/release-notes/nuget-5.1-rtm) is now
included with Visual Studio for Mac 8.2.

## Support partial installs into multi-target framework projects

A NuGet package being installed into a multi-target framework project
may not support all frameworks. When this happens the PackageReference
will now be added with a condition so the NuGet package is not used
for frameworks that are not supported.

```
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <TargetFrameworks>net472;netstandard1.2</TargetFrameworks>
  </PropertyGroup>

  <ItemGroup Condition="'$(TargetFramework)' == 'net472'">
    <PackageReference Include="EntityFramework" Version="6.2.0" />
  </ItemGroup>
</Project>
```

## Bug Fixes

**Fixed no network error displayed in Add Packages dialog**

Networking errors were not being reported in the Add Packages dialog
resulting in no information being available explaining why there were
no search results.

Visual Studio for Mac uses Xamarin.Mac's NSUrlSessionHandler
for HTTP requests and the NSUrlSessionHandler reports errors
using curly braces. For example:

```
[nuget.org] The Internet connection appears to be offline.
  Error Domain=NSURLErrorDomain Code=-1009 "The Internet connection appears to be offline." UserInfo={NSUnderlyingError=0x7fce42892a80 {Error Domain=kCFErrorDomainCFNetwork Code=-1009 "(null)" UserInfo={_kCFStreamErrorCodeKey=50, _kCFStreamErrorDomainKey=1}}, NSErrorFailingURLStringKey=https://api-v2v3search-1.nuget.org/query?q=&skip=0&take=26&prerelease=false&supportedFramework=Xamarin.iOS,Version=v1.0&semVerLevel=2.0.0, NSErrorFailingURLKey=https://api-v2v3search-1.nuget.org/query?q=&skip=0&take=26&prerelease=false&supportedFramework=Xamarin.iOS,Version=v1.0&semVerLevel=2.0.0, _kCFStreamErrorDomainKey=1, _kCFStreamErrorCodeKey=50, NSLocalizedDescription=The Internet connection appears to be offline.}
```

This text was then being passed to string.Format in the Add Packages
dialog and was causing a FormatException. The error would then not be
displayed in the Add Packages dialog. The text now has the curly braces
escaped to prevent the FormatException.

**Fixed restoring too many projects when target framework changed**

When the target framework was changed in a SDK style project all SDK style
projects in the solution were restored. Now only the project and
projects that depend on it are restored.

**Fixed adding non-sdk style project triggering NuGet restore multiple times**

Adding a new non-sdk style project to a solution could cause a NuGet
restore to run multiple times.

The problem was that the SDK style projects, such as those that target
Xamarin.Mac, may change their target framework on re-evaluation. This
was causing Visual Studio for Mac
to run a restore for all SDK style projects in the solution, multiple
times. To avoid this the target framework changes during re-evaluation
are ignored.

**Fixed restore menu incorrectly disabled**

In the Solution window on right clicking a solution, or the packages folder, the restore
menu was incorrectly disabled when an SDK style project targeted .NET Framework
or when the project had its RestoreProjectStyle set to PackageReference and
had no PackageReferences.
