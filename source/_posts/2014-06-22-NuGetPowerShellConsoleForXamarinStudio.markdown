---
layout: post
title: "NuGet PowerShell Console for Xamarin Studio"
date: 2014-06-22 21:05
comments: true
categories: NuGet Xamarin MonoDevelop
---

Xamarin Studio 5.0 now has [NuGet support built in](http://lastexitcode.com/blog/2014/06/01/NuGetSupportInXamarinStudio5-0/). There are also some experimental NuGet features that are available for Xamarin Studio 5.0 and MonoDevelop 5.0 as part of a separate [NuGet extensions addin](https://github.com/mrward/monodevelop-nuget-extensions). One such experimental feature is a new PowerShell console window that provides Xamarin Studio with the standard NuGet commands and also works on the Mac, not just Windows.

{% img /images/blog/NuGetPowerShellConsoleForXamarinStudio/PackageConsoleExtensionWindow.png 'NuGet PowerShell Console Window' 'NuGet PowerShell Console Window' %}  

The new console window is powered by Pash. Pash is a open source reimplementation of PowerShell that works with Mono and runs on Windows, Mac and Linux. It was created by [Igor Moochnick](https://twitter.com/igor_moochnick) and more recently development has been restarted by [Jay Bazuzi](https://twitter.com/jaybazuzi).

## PowerShell Console Features

   * Standard NuGet PowerShell commands available - Install-Package, Update-Package, Uninstall-Package, Get-Project and Get-Package
   * Integrates with the existing NuGet addin that ships with Xamarin Studio.
   * Cross platform - works on Windows and Mac.
   * Powered by [Pash](https://github.com/Pash-Project/Pash).
   * Partial implementation of the [Visual Studio object model EnvDTE](http://msdn.microsoft.com/en-us/library/envdte.aspx).

## Limitations

   * PowerShell support is not fully implemented in Pash so PowerShell scripts may not fully work.
   * Xamarin Studio needs to be closed and re-opened before the console can be used. Otherwise the Xamarin Studio will crash.
   * Some standard parameters are missing from the NuGet commands (e.g. -verbose).
   * The NuGet commands are not fully up to date compared with the commands available in Visual Studio. One example is **Update-Package -Reinstall** which is currently not supported.

## Installation

The NuGet extensions addin is available from the [MonoDevelop addin repository](http://addins.monodevelop.com/Project/Index/121). To install the addin:

 * Open the **Add-in Manager** dialog.
 * Select the **Gallery** tab.
 * Select **Xamarin Studio Add-in Repository (Alpha channel)** from the drop down list.
 * Expand **IDE extensions**.
 * Select **NuGet Package Management Extensions**.
 * Click the **Refresh** button if the addin is not visible.
 * Click **Install...** to install the addin.

Please close and re-open Xamarin Studio before trying to open the PowerShell Console window otherwise Xamarin Studio will crash.

## Using the PowerShell Console

To open the console window, from the **View** menu select **Pads** and then select **Package Console Extension**.

{% img /images/blog/NuGetPowerShellConsoleForXamarinStudio/ViewPackageConsoleExtensionPadMenuItem.png 'View - Package Console Extension menu item' 'View - Package Console Extension menu item' %}  

The top of the console window has drop downs allowing you to choose a different default package source or default project. At the top right there is a Clear Console button which will clear all the text from the console.

{% img /images/blog/NuGetPowerShellConsoleForXamarinStudio/PackageConsoleExtensionWindowInstallingJsonNet.png 'PowerShell Console Window - Installing Json.NET' 'PowerShell Console Window - Installing Json.NET' %}  

The standard [NuGet PowerShell commands](http://docs.nuget.org/docs/reference/package-manager-console-powershell-reference) are available from the console window:

 * Install-Package
 * Uninstall-Package
 * Update-Package
 * Get-Package
 * Get-Project

There is also a partial implementation of the [Visual Studio object model (EnvDTE)](http://msdn.microsoft.com/en-us/library/envdte.aspx) which you can use from the console and from PowerShell scripts inside a NuGet package. Currently the Solution object and the CodeModel is not fully implemented when compared with SharpDevelop.

{% img /images/blog/NuGetPowerShellConsoleForXamarinStudio/PackageConsoleExtensionWindowUsingEnvDTEProjectModel.png 'PowerShell Console Window - Using EnvDTE project model' 'PowerShell Console Window - Using EnvDTE project model' %}  

When you install a NuGet package from the console and the NuGet package has an **init.ps1** or an **install.ps1** PowerShell script then these scripts will be run. Similarly on uninstalling a NuGet package if it contains an **uninstall.ps1** script then this will be run in the console. However there are some areas of PowerShell that are not fully implemented in Pash so the script may well fail. The failure will be logged in the console window but it will not prevent the package from being installed or uninstalled.

That concludes the introduction to the experimental NuGet Package Console in Xamarin Studio.