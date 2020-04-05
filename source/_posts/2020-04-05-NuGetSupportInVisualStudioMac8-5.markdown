---
layout: post
title: "NuGet Support in Visual Studio for Mac 8.5"
date: 2020-04-05 12:00
comments: true
categories: NuGet Xamarin VSMac MonoDevelop
---

## Changes

   * NuGet 5.4 support
   * Support Plugin-based NuGet Credential Providers
   * Fixed requests bypassing the native macOS HTTP handler
 
More information on all the new features and changes in [Visual Studio for Mac 8.5](https://www.visualstudio.com/vs/visual-studio-mac/)
can be found in the [release notes](https://docs.microsoft.com/en-us/visualstudio/releasenotes/vs2019-mac-relnotes).

## NuGet 5.4 support
    
[NuGet 5.4.0.6315](https://docs.microsoft.com/en-us/nuget/release-notes/nuget-5.4) is now
included with Visual Studio for Mac 8.5.

## Support Plugin-based NuGet Credential Providers

[NuGet credential provider plugins](https://docs.microsoft.com/en-us/nuget/reference/extensibility/nuget-cross-platform-plugins)
are now supported by Visual Studio for Mac.

Plugins are self-contained executables which provide
credentials for NuGet package sources. One example is the
[Azure Artifacts Credential Provider](https://github.com/microsoft/artifacts-credprovider) which
Visual Studio for Mac can now use whilst it does not have integrated support for
accessing packages sources available from
[Azure DevOps](https://docs.microsoft.com/en-us/azure/devops/artifacts/get-started-nuget).

## Bug Fixes

**Fixed HTTP requests bypassing the native macOS HTTP handler**

Restoring or installing a NuGet package into a project that uses
PackageReferences was bypassing the native macOS HTTP handler.

This was due
to NuGet's CachingSourceProvider creating its own set of
INuGetResourceProviders when it creates a set of new SourceRepository
instances. This resulted in the default HttpSourceResourceProvider
being used, instead of the custom one provided by Visual Studio for Mac.
The default HttpSourceResourceProvider uses Mono's HttpClientHandler not
the native HTTP Client handle NSUrlSessionHandler provided by 
Xamarin.Mac. This would happen
when the Nuget package was was downloaded.

NuGet 5.4 now supports defining custom INuGetResourceProvider factory that is used
globally. This is now used by Visual Studio for Mac so all HTTP requests
use the native macOS HTTP handler NSUrlSessionHandler.
