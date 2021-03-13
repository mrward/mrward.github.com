---
layout: post
title: "NuGet Support in Visual Studio for Mac 8.9"
date: 2021-03-13 16:20
comments: true
categories: NuGet Xamarin VSMac MonoDevelop
---

## Changes

   * Fixed implicit NuGet packages being updated
 
More information on all the new features and changes in [Visual Studio for Mac 8.9](https://www.visualstudio.com/vs/visual-studio-mac/)
can be found in the [release notes](https://docs.microsoft.com/en-us/visualstudio/releasenotes/vs2019-mac-relnotes).

**Fixed implicit NuGet packages being updated**

Updating all NuGet packages, from the Solution window, in a .NET Core
2.1 project would fail with an error.

```
Package Microsoft.NETCore.App 2.2.8 is not compatible with netcoreapp2.1 (.NETCoreApp,Version=v2.1). Package Microsoft NETCore.App 2.2.8 supports: netcoreapp2.2 (.NETCoreApp,Version=v2.2)
```

The problem was that implicitly defined (or auto-referenced) NuGet
package references were being updated when they should be ignored since
they cannot be updated. Now Visual Studio for Mac does not attempt to
update implicitly defined NuGet package references.