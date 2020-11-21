---
layout: post
title: "NuGet Support in Visual Studio for Mac 8.8"
date: 2020-11-21 14:20
comments: true
categories: NuGet Xamarin VSMac MonoDevelop
---

## Features

   * Azure DevOps NuGet package source authentication with signed in account
   * NuGet 5.8 support
 
More information on all the new features and changes in [Visual Studio for Mac 8.8](https://www.visualstudio.com/vs/visual-studio-mac/)
can be found in the [release notes](https://docs.microsoft.com/en-us/visualstudio/releasenotes/vs2019-mac-relnotes).

## Azure DevOps NuGet package source authentication with signed in account

Visual Studio for Mac now has integrated support for accessing 
[Azure DevOps NuGet package sources](https://docs.microsoft.com/en-us/azure/devops/artifacts/nuget/consume?view=azure-devops) 
using the [account signed into the IDE](https://docs.microsoft.com/en-us/visualstudio/mac/signing-in?view=vsmac-2019) 
without requiring a personal access token (PAT).

{% img /images/blog/NuGetSupportInVisualStudioMac8-8/AzureDevOps-NuGetPackageSource.gif 'Azure DevOps NuGet package source prompting for sign in in Visual Studio for Mac' 'Azure DevOps NuGet package source prompting for sign in in Visual Studio for Mac' %}

If you are not signed in then the
account sign in dialog will be opened when an Azure DevOps NuGet package source
is accessed that requires authentication.

## NuGet 5.8 support
    
[NuGet 5.8.0.6860](https://docs.microsoft.com/en-us/nuget/release-notes/nuget-5.8) is now
included with Visual Studio for Mac 8.8.
