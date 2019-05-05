---
layout: post
title: "NuGet PowerShell Core Console in Visual Studio for Mac 8.0"
date: 2019-05-05 12:40
comments: true
categories: NuGet Xamarin VSMac MonoDevelop
---

The [NuGet extensions addin](https://github.com/mrward/monodevelop-nuget-extensions) now includes a PowerShell
Core based NuGet Package Manager console for Visual Studio for Mac 8.0.

{% img /images/blog/NuGetPowerShellCoreConsoleVisualStudioMac8-0/NuGetPackageManagerConsoleWindow.png 'NuGet Package Manager Console' 'NuGet Package Manager Console window' %}

A [NuGet PowerShell console](https://lastexitcode.com/blog/2014/06/22/NuGetPowerShellConsoleForXamarinStudio/) has been available with 
the [NuGet extensions addin](https://github.com/mrward/monodevelop-nuget-extensions) since
Xamarin Studio 5.0. Previously it used [Pash](https://github.com/Pash-Project/Pash), a cross-platform, open source reimplementation
of PowerShell. Pash is no longer being actively developer on after Microsoft released [PowerShell Core](https://github.com/PowerShell/PowerShell).
The PowerShell support in Pash was incomplete, so whilst the NuGet commands, such as Install-Package, were supported, more
complicated commands, such as those provided by [Entity Framework Core](https://docs.microsoft.com/en-us/ef/core/), were not supported. 
Moving to PowerShell Core provides full PowerShell support, and with a partial implementation of the Visual Studio EnvDTE API,
[Entity Framework Core commands](https://docs.microsoft.com/en-us/ef/core/miscellaneous/cli/powershell) are now supported.

The NuGet extensions addin runs a .NET Core console 
application that hosts PowerShell Core. Communication between
the addin running in Visual Studio for Mac and the console application is through the 
[StreamJsonRpc](https://github.com/Microsoft/vs-streamjsonrpc) library.

## New Features

The following lists the new features compared with the Pash based PowerShell console.

 - PowerShell Core based NuGet Package Manager Console
    - Full PowerShell support
    - Get-Help now supported
 - Support for Entity Framework Core NuGet commands
 - Tab completion
 - Support for stopping the executing command
 - Opening the NuGet package source configuration page from the console

## Limitations

 - **Visual Studio EnvDTE API implementation is incomplete**

 The Visual Studio EnvDTE API is partially implemented. Whilst Entity Framework Core is 
 supported other PowerShell scripts included with NuGet packages may not work.

 - **Password protected NuGet package sources not supported in the following PowerShell commands:**

   - Find-Package
   - Get-Package

Note that the other NuGet PowerShell commands are supported.

The NuGet extensions addin will not send usernames and passwords to the PowerShell Core
host console application. .NET Core also does not support decrypting passwords stored in the
NuGet.Config file.

The Find-Package and Get-Package commands all run completely within PowerShell hosted in the .NET Core
console application.

The other commands work since they run partially within Visual for Mac where actions involving password protected
NuGet package sources are supported.

 - **Requires .NET Core 2.1 SDK to be installed**

## Opening the NuGet Package Manager Console window

From the View menu, select Pads, then select NuGet Package Manager Console.

{% img /images/blog/NuGetPowerShellCoreConsoleVisualStudioMac8-0/ViewPadsNuGetPackageManagerConsoleMenu.png 'View - Pages - NuGet Package Manager Console menu' 'View - Pages - NuGet Package Manager Console menu' %}

## Entity Framework Core Support

The PowerShell commands provided by the
[Microsoft.EntityFrameworkCore.Tools NuGet package](https://docs.microsoft.com/en-us/ef/core/miscellaneous/cli/powershell)
are supported in .NET Core projects.

 - Add-Migration
 - Drop-Database
 - Get-DbContext
 - Remove-Migration
 - Scaffold-DbContext
 - Script-Migration
 - Update-Database

{% img /images/blog/NuGetPowerShellCoreConsoleVisualStudioMac8-0/EntityFrameworkCoreMigrationPowerShellCommands.png 'Package Manager Console - Entity Framework Core Add-Migration and Update-Database commands' 'Package Manager Console - Entity Framework Core Add-Migration and Update-Database commands' %}

Note that the Entity Framework Core commands are not currently supported in projects that target the .NET Framework. This
is because the PowerShell commands attempt to directly run ef.exe that is included in the NuGet package instead of using the .NET Core ef.dll
which is used with projects that target .NET Core.

## Tab Completion

Pressing tab in the console window will try to auto-complete the command being typed in.

{% img /images/blog/NuGetPowerShellCoreConsoleVisualStudioMac8-0/TabCompletionNewtonsoftJson.png 'Tab completion - Install-Package Newtonsoft.Json' 'Tab completion - Install-Package Newtonsoft.Json' %}

{% img /images/blog/NuGetPowerShellCoreConsoleVisualStudioMac8-0/TabCompletionPackageVersion.png 'Tab completion - Install-Package Newtonsoft.Json -Version' 'Tab completion - Install-Package Newtonsoft.Json -Version' %}

If there is only one match on pressing tab then the text will be inserted.

If there are multiple possible matches then a window will be displayed allowing an
item to be selected by pressing Tab, Enter or Return. Typing with this completion list
window open will filter the items in the list.

## Stopping the PowerShell command being run

When a PowerShell command is being run the Stop button is enabled. The Stop button is the last button on the top right of the NuGet
Package Console window.

{% img /images/blog/NuGetPowerShellCoreConsoleVisualStudioMac8-0/PackageManagerConsoleStopButton.png 'Package Manager Console stop button' 'Package Manager Console stop button' %}

Clicking this button will attempt to stop the PowerShell command being run.

{% img /images/blog/NuGetPowerShellCoreConsoleVisualStudioMac8-0/PackageManagerConsolePipelineStopped.png 'Package Manager Console pipeline stopped' 'Package Manager Console pipeline stopped' %}

## Opening the NuGet Package Console configuration page

At the top of the NuGet Package Console window there is a cog icon after the list of package sources.

{% img /images/blog/NuGetPowerShellCoreConsoleVisualStudioMac8-0/PackageManagerConsoleCogIcon.png 'Package Manager Console configure sources - cog icon' 'Package Manager Console configure sources cog icon' %}

Clicking this cog icon will open the NuGet Package Sources configuration
page which is also available from Preferences - NuGet - Sources.

## Installation

The NuGet extensions addin is available from the MonoDevelop addin repository. To install the addin:

 - From the main menu, open the Extensions Manager dialog.
 - Select the Gallery tab.
 - Expand IDE extensions.
 - Select NuGet Package Management Extensions.
 - Click the Refresh button if the addin is not visible.
 - Click Install… to install the addin.
 