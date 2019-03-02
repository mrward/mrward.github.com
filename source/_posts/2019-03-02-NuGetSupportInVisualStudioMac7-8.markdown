---
layout: post
title: "NuGet Support in Visual Studio for Mac 7.8"
date: 2019-03-02 12:40
comments: true
categories: NuGet Xamarin VSMac MonoDevelop
---

## Changes

   * NuGet package diagnostics displayed in Solution window
   * Support restoring multi-target framework projects
   * Display summary of NuGet restore errors in Package Console
   * Fixed wrong version of Microsoft.AspNetCore.App being restored
   * Fixed Paket restore not working with SDK style projects
   * Fixed NuGet restore ignoring build targets

More information on all the new features and changes in [Visual Studio for Mac 7.8](https://www.visualstudio.com/vs/visual-studio-mac/)
can be found in the [release notes](https://docs.microsoft.com/en-us/visualstudio/releasenotes/vs2017-mac-relnotes#whats-new-in-78).

## NuGet package diagnostics displayed in Solution window

NuGet package diagnostic warnings are now shown in the Solution window.

{% img /images/blog/NuGetSupportInVisualStudioMac7-8/NuGetPackageDiagnosticWarningInSolutionWindow.png 'NuGet package diagnostic warnings in Solution Window' 'NuGet package diagnostic warnings in Solution Window' %}

The diagnostic warnings are shown underneath the NuGet package with
a warning icon. Hovering over the warning icon will show a tooltip.

{% img /images/blog/NuGetSupportInVisualStudioMac7-8/NuGetPackageDiagnosticWarningTooltip.png 'NuGet package diagnostic warning tooltip in Solution Window' 'NuGet package diagnostic warning tooltip in Solution Window' %}

## Support restoring multi-target framework projects

Projects that have multiple target frameworks now have all frameworks restored.

{% img /images/blog/NuGetSupportInVisualStudioMac7-8/MultiTargetProjectInSolutionWindow.png 'Multi-target framework project restored in Solution window' 'Multi-target framework project restored in Solution window' %}

Previously only the first target framework would be restored.

Conditional PackageReferences defined in the project file are also now
respected when restoring the project. Previously the conditions on the
PackageReferences would be ignored.

{% img /images/blog/NuGetSupportInVisualStudioMac7-8/ConditionalPackageReferences.png 'Conditional PackageReferences restored in Solution window' 'Conditional PackageReferences restored in Solution window' %}

Visual Studio for Mac now uses the GenerateRestoreGraphFile MSBuild target to determine
package dependencies. Previously this information was obtained from the project information
held in memory. This fixes several NuGet restore bugs in Visual Studio for Mac.

## Display summary of NuGet restore errors in Package Console

Creating an xUnit .NET Core test project named 'xunit' fails to
restore since there is a package reference cycle between the project
and the xunit NuGet package. Whilst this reference cycle is reported it is
hidden in the Package Console output and all you would see was a message
indicating that the restore had failed. Now the error information is
shown at the end of the
Package Console as a summary of the failures to make it easier to
see the problem.

Now for the xunit project you will see the following
at the end of the Package Console output:

    Cycle detected.
      xunit -> xunit (>= 2.3.1).
    Restore failed.

## Bug Fixes

**Fixed wrong version of Microsoft.AspNetCore.App being restored**

With an ASP.NET Core 2.1 project, that had included a PackageReference for
Microsoft.AspNetCore.App version 2.1.5, Visual Studio for Mac would incorrectly restore
Microsoft.AspNetCore.App version 2.1.1.

Using the GenerateRestoreGraphFile MSBuild target to determine package reference
information when restoring has fixed this problem.

**Fixed Paket restore not working with SDK style projects**

When Paket is used with an SDK style project it injects PackageReferences
via the PaketRestore target from the Paket.Restore.targets file. Visual Studio for
Mac was not using MSBuild to get the package reference information so
PackageReferences defined by Paket were not being restored or made available.

**Fixed NuGet restore ignoring build targets**

OrchardCore would fail to restore when opened in Visual Studio for Mac. OrchardCore
defines the PackageReference versions in a separate MSBuild .props file and has a custom
MSBuild target to define these versions. This custom MSBuild target is now supported
since Visual Studio for Mac uses the GenerateRestoreGraphFile MSBuild target to
determine package references when restoring.
