---
layout: post
title: "NuGet Support in Visual Studio for Mac 17.0"
date: 2022-06-19 12:20
comments: true
categories: NuGet Xamarin VSMac MonoDevelop
---

## Changes

   * NuGet 6.0 support
   * NuGet.Config file location changed
   * NuGet package source passwords now stored in the Keychain
   * Improved clear text password handling for NuGet package sources
   * Support NuGet restore with .NET MSBuild and MSBuild on mono
   * NuGetizer support removed
 
More information on all the new features and changes in [Visual Studio for Mac 17.0](https://www.visualstudio.com/vs/visual-studio-mac/)
can be found in the [release notes](https://docs.microsoft.com/en-us/visualstudio/releases/2022/mac-release-notes).

## NuGet 6.0 support
    
[NuGet 6.0.0.262](https://docs.microsoft.com/en-us/nuget/release-notes/nuget-6.0) is now
included with Visual Studio for Mac 17.0.

## NuGet.Config file location changed

Visual Studio for Mac 17.0 now runs on .NET 6 instead of Mono. This means the
NuGet.Config file location has changed.

Old NuGet.Config file location used by Visual Studio for Mac 8.10 and mono:

    ~/.config/NuGet/NuGet.Config

New NuGet.Config file location used by Visual Studio for Mac 17.0 and the dotnet command line interface:

    ~/.nuget/NuGet/NuGet.Config

## NuGet package source passwords now stored in the Keychain

The dotnet command line interface does not support encrypting NuGet package source
passwords.

This is because the 
[ProtectedData](https://docs.microsoft.com/en-us/dotnet/api/system.security.cryptography.protecteddata?view=dotnet-plat-ext-6.0) class
used by NuGet to encrypt and decrypt the package source passwords 
is not available for .NET. The ProtectedData class is available for mono and .NET Framework v4.

One workaround is to 
[store the passwords in clear text](https://docs.microsoft.com/en-us/nuget/reference/nuget-config-file#packagesourcecredentials)
in the NuGet.Config file.

This can be done via [dotnet nuget add source](https://docs.microsoft.com/en-us/dotnet/core/tools/dotnet-nuget-add-source).

    dotnet nuget add source https://someServer/myTeam -n myTeam -u myUsername -p myPassword --store-password-in-clear-text

Or by editing the ~/.nuget/NuGet/NuGet.Config file.

```
<configuration>
  <packageSources>
    <add key="myTeam" value="https://someServer/myTeam" />
  </packageSources>
  <packageSourceCredentials>
    <myTeam>
        <add key="Username" value="myUsername" />
        <add key="ClearTextPassword" value="myPassword" />
      </myTeam>
  </packageSourceCredentials>
</configuration>
```

Now that Visual Studio for Mac 17.0 runs on .NET 6 it is also affected by
NuGet not being able to store encrypted passwords in the NuGet.Config file.

If a NuGet package source with a password is created in Visual Studio 
for Mac 17.0 the password will be stored in the Keychain rather than using
a clear text password.

{% img /images/blog/NuGetSupportInVisualStudioMac17-0/AddNuGetPackageSourceCredential.png 'Add NuGet package source with password in preferences' 'Add NuGet package source with password in preferences' %}

{% img /images/blog/NuGetSupportInVisualStudioMac17-0/KeychainNuGetPackageSourcePassword.png 'Package source password saved in Keychain' 'Package source password saved in Keychain' %}

## Improved clear text password handling for NuGet package sources

In Visual Studio for Mac 8.10 when the package sources where
saved in Preferences - NuGet - Sources any clear text passwords
were encrypted in the saved NuGet.Config file.

In Visual Studio for Mac 17.0 package sources that use clear text
passwords will not have their passwords encrypted in the NuGet.Config file.

## Support NuGet restore with .NET MSBuild and MSBuild on mono

In order to support classic Xamarin projects, and projects that require mono,
Visual Studio for Mac 17.0 supports MSBuild on Mono and dotnet msbuild.

If any project in a solution has mono as a target
runtime then the solution is restored with MSBuild on mono. If all
projects target the dotnet runtime then dotnet's MSBuild will be
used. This prevents the restore failing for classic Android and iOS
projects since dotnet's MSBuild cannot resolve the Xamarin MSBuild targets
files, causing errors similar to the following:

    error MSB4019: The imported project "/usr/local/share/dotnet/sdk/6.0.101/Xamarin/Android/Xamarin.Android.CSharp.targets" was not found.
    Confirm that the expression in the Import declaration

If the solution is configured to build with MSBuild on Mono then
large solutions are restored with MSBuild on Mono.

## NuGetizer support removed

Integrated support for [NuGetizer](https://github.com/NuGet/NuGet.Build.Packaging) has been 
removed from Visual Studio for Mac 17.0. This is because the NuGetizer project is no
longer being maintained. 

[dotnet pack](https://docs.microsoft.com/en-us/dotnet/core/tools/dotnet-pack) can be used as
an alternative.