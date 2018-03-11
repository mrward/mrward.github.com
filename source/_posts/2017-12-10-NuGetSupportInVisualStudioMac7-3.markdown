---
layout: post
title: "NuGet Support in Visual Studio for Mac 7.3"
date: 2017-12-10 11:00
comments: true
categories: NuGet Xamarin VSMac MonoDevelop
---

## Changes

   * Support installing System.ValueTuple NuGet Package from Quick Fixes
   * NuGetizer 3000 - Update NuGet.Build.Packaging to version 0.2
   * Support NuGet Restore MSBuild Properties
     * RestoreAdditionalProjectFallbackFolders
     * RestoreAdditionalProjectSources
     * RestoreFallbackFolders
     * RestorePackagesPath
     * RestoreSources
   * Fixed transitive dependencies not available when using project.json

More information on all the new features and changes in [Visual Studio for Mac 7.3](https://www.visualstudio.com/vs/visual-studio-mac/) can be found in the [release notes](https://www.visualstudio.com/en-us/news/releasenotes/vs2017-mac-relnotes#release-date-december-4-2017---visual-studio-2017-version-73-730797).

## Quick Fix - Install System.ValueTuple NuGet Package

[C# 7.0 introduced tuple types](https://blogs.msdn.microsoft.com/dotnet/2016/08/24/whats-new-in-csharp-7-0/)
that may require the System.ValueTuple NuGet package
to be added to the project. Visual Studio for Mac
now offers to install the System.ValueTuple NuGet package if the project requires this NuGet package.

Creating a .NET Framework 4.6 project with the following code:

```
	public (string, int) GetValues(string id)
	{
		return ("Name", 25);
	}
```

You will see error markers in the text editor. If you hover over 
one of these error markers you will see a
**Predefined type 'System.ValueTuple`2' is not defined or imported** message.

{% img /images/blog/NuGetSupportInVisualStudioMac7-3/ValueTupleTextEditorErrorTooltip.png 'ValueTuple type not defined text editor error tooltip' 'ValueTuple type not defined text editor error tooltip' %}

If you right click the error marker there is now a new
**Install Package 'System.ValueTuple'** Quick Fix action available.

{% img /images/blog/NuGetSupportInVisualStudioMac7-3/TextEditorInstallSystemValueTuplePackageQuickFix.png 'Text editor Install System.ValueTuple Package quick fix menu item' 'Text editor Install System.ValueTuple Package quick fix menu item' %}

From this menu you can install the latest System.ValueTuple NuGet package
or open the Add Packages dialog to search for a specific version of the System.ValueTuple NuGet
package.

{% img /images/blog/NuGetSupportInVisualStudioMac7-3/TextEditorInstallLatestSystemValueTupleQuickFix.png 'Text editor install latest System.ValueTuple package quick fix menu item' 'Text editor install latest System.ValueTuple package quick fix menu item' %}

Note that if the project is targeting .NET Framework 4.7 or .NET Standard 2.0 then the
System.ValueTuple NuGet package is not required.

Also note that the official nuget.org package source needs to be available
for this feature to work.

## NuGetizer 3000 - Updated NuGet.Build.Packaging to version 0.2

The version of the NuGet.Build.Packaging NuGet package used by default for NuGetizer 3000 support
has been updated from 0.1.276 to 0.2.0. This fixes a
potential build error when building with Mono 5.6.

```
Using "ReportAssetsLogMessages" task from assembly "/usr/local/share/dotnet/sdk/2.0.0/Sdks/Microsoft.NET.Sdk/build/../tools/net46/Microsoft.NET.Build.Tasks.dll".
Task "ReportAssetsLogMessages"
/usr/local/share/dotnet/sdk/2.0.0/Sdks/Microsoft.NET.Sdk/build/Microsoft.PackageDependencyResolution.targets(323,5): error MSB4018: The "ReportAssetsLogMessages" task failed unexpectedly. [RefactoringEssentials/RefactoringEssentials.2017/RefactoringEssentials.csproj]
/usr/local/share/dotnet/sdk/2.0.0/Sdks/Microsoft.NET.Sdk/build/Microsoft.PackageDependencyResolution.targets(323,5): error MSB4018: System.TypeLoadException: Could not resolve type with token 0100005b (from typeref, class/assembly NuGet.ProjectModel.IAssetsLogMessage, NuGet.ProjectModel, Version=4.3.0.5, Culture=neutral, PublicKeyToken=31bf3856ad364e35) [RefactoringEssentials/RefactoringEssentials.2017/RefactoringEssentials.csproj]
/usr/local/share/dotnet/sdk/2.0.0/Sdks/Microsoft.NET.Sdk/build/Microsoft.PackageDependencyResolution.targets(323,5): error MSB4018: at Microsoft.NET.Build.Tasks.TaskBase.Execute () [0x00000] in <01420900fd004c128de2d2ee31bad624>:0 [RefactoringEssentials/RefactoringEssentials.2017/RefactoringEssentials.csproj]
/usr/local/share/dotnet/sdk/2.0.0/Sdks/Microsoft.NET.Sdk/build/Microsoft.PackageDependencyResolution.targets(323,5): error MSB4018: at Microsoft.Build.BackEnd.TaskExecutionHost.Microsoft.Build.BackEnd.ITaskExecutionHost.Execute () [0x00023] in <765502eb2f884ce79731edeb4b0517fb>:0 [RefactoringEssentials/RefactoringEssentials.2017/RefactoringEssentials.csproj]
/usr/local/share/dotnet/sdk/2.0.0/Sdks/Microsoft.NET.Sdk/build/Microsoft.PackageDependencyResolution.targets(323,5): error MSB4018: at Microsoft.Build.BackEnd.TaskBuilder+d__26.MoveNext () [0x0022d] in <765502eb2f884ce79731edeb4b0517fb>:0 [RefactoringEssentials/RefactoringEssentials.2017/RefactoringEssentials.csproj]
```

The ReadLegacyDependencies target from NuGet.Build.Packaging.Tasks is called when PackOnBuild
is set to true.

The NuGet.Build.Packaging.Tasks assembly was loading an embedded NuGet.ProjectModel
assembly that conflicts with the version that is used by Microsoft.NET.Build.Tasks.

## Support RestoreAdditionalProjectFallbackFolders MSBuild Property

The RestoreAdditionalProjectFallbackFolders MSBuild property is
read and appended to the list of fallback folders stored in the
project.assets.json file. The .NET Core 2.0 SDK will set this
MSBuild property to point to the NuGet package fallback folder
that is installed with the SDK. This will be used
to resolve NuGet packages first before downloading them to the
~/.nuget/packages folder.

## Support RestoreAdditionalProjectSources MSBuild Property

The RestoreAdditionalProjectSources MSBuild property can be used
to add additional package sources to the existing list of sources
used to resolve packages.

## Support RestoreFallbackFolders MSBuild Property

The RestoreFallbackFolders MSBuild property can be used by project that uses
PackageReferences to define a set of package fallback folders that
will override any specified in the NuGet.Config file. It can also
be used to clear any pre-defined fallback folders by specifying
'clear' as its value. Note that this value does not affect any
folders defined by RestoreAdditionalProjectFallbackFolders which
will be appended even if RestoreFallbackFolders is set to 'clear'.

## Support RestorePackagesPath MSBuild Property

The RestorePackagesPath MSBuild property can be used to override the global packages
folder location when a project uses a PackageReference.

## Support RestoreSources MSBuild property

The RestoreSources MSBuild property can be used to override
the sources defined by any NuGet.Config file. Any sources defined in the
RestoreAdditionalProjectSources MSBuild property will still be appended to the
list of sources if RestoreSources is defined.

## Bug Fixes

**Transitive dependencies not available when using project.json**

With two projects A and B that both use project.json files, project B
referencing project A, the NuGet package dependencies used by project
A were not available to project B transitively. This was because the
project reference information was not being used
when a restore occurred. This caused the assemblies from the transitively
referenced NuGet packages to not be added to the
project.lock.json file for Project B.

**Unable to debug a new Azure Functions project without re-opening project**

After creating a new Azure Functions project it was possible to build the
project but not to debug it or run it. On closing and re-opening the solution
it would be possible to debug and run the Azure Functions project.

When Visual Studio for Mac created a new Azure Functions project
it initially determined that it could not run or debug the project.
After the Azure Functions project has its PackageReferences restored
it gains an AzureFunctions project capability. This capability is used to
determine if the project can be run. This change in the capability
was not handled by Visual Studio for Mac so it did not allow the
project to be run until the project was closed and re-opened.

Now, after the NuGet package restore is finished, the project is
re-evaluated and a check is made to see if the project can now be run.

**Fix generated NuGet files being imported twice**
    
The generated ProjectName.nuget.g.targets and ProjectName.nuget.g.props
that are created for .NET Core projects in the base intermediate
directory were being imported twice when evaluating the project.
Once by Microsoft.Common.props, provided with Mono, and once
by Visual Studio for Mac.
    
This duplicate import was resulting in a duplicate file being added
to the project held in memory by Visual Studio for Mac when the
Xamarin.Forms 2.4 NuGet package was used in a .NET Standard
project and no .NET Core SDK is installed. This would result in the
content page xaml and associated C# file not being nested in the
solution window.