---
layout: post
title: ".NET Core Support in Visual Studio for Mac 7.6"
date: 2018-09-02 11:00
comments: true
categories: NETCore Xamarin VSMac MonoDevelop
---

## Changes

  * NuGet SDK resolver now used to find NuGet SDK packages
  * Fixed Synchronous operation cancelled message on stopping debugging
  * Fixed debugger hanging when debugging unit tests
  * Fixed command line arguments not used with a new project
  * Fixed hang when discovering unit tests
  * Allow loading of SDK style projects without a main PropertyGroup
  * Fixed TargetFramework not being updated in project
  * Fixed .xaml.cs code completion in new Xamarin.Forms .NET Standard project
  * Fixed default build action for new HTML file
  * Fixed SDK version not being parsed from Sdk attribute
  * Fixed null reference on opening .NET Core project with sdk version
  * Fixed editor errors when .NET Standard assembly referenced in Xamarin.iOS project

More information on all the new features and changes in [Visual Studio for Mac 7.6](https://www.visualstudio.com/vs/visual-studio-mac/)
can be found in the [release notes](https://docs.microsoft.com/en-us/visualstudio/releasenotes/vs2017-mac-relnotes#whats-new-in-76).

## NuGet SDK resolver now used to find NuGet SDK packages

Visual Studio for Mac now has support for the 
[NuGet SDK resolver](https://github.com/Microsoft/msbuild/issues/2803). The NuGet
SDK resolver will download and install SDKs for SDK style
projects if these SDKs are missing.

    <Project Sdk="My.Custom.Sdk/2.3.4">
      ...
    </Project>

The SDK resolution is done in the background
when the project is opened and there is currently no visual indication that
this is happening.

The NuGet library assemblies are not available to the remote MSBuild host used by
Visual Studio for Mac so the NuGet SDK resolver was previously failing to load. The NuGet SDK
resolver supports a MSBUILD_NUGET_PATH environment variable which is now set by
Visual Studio for Mac to point to the directory containing the NuGet assemblies
that are included with the IDE.

## Bug Fixes

**Fixed synchronous operation cancelled message on stopping debugging**

Stopping the .NET Core debugger would sometimes result in
a dialog being displayed indicating that the debugger operation failed.

{% img /images/blog/NetCoreSupportInVisualStudioMac7-6/DebuggerOperationFailedMessage.png 'Debugger operation failed - Synchronous operation cancelled - dialog' 'Debugger operation failed - Synchronous operation cancelled - dialog' %}

**Fixed debugger hanging when debugging unit tests**

The .NET Core debugger would sometimes hang Visual Studio for Mac when debugging unit tests.
The problem was that if the breakpoint was placed on an invalid line then the .NET Core debugger
would send back the adjusted breakpoint location. Visual Studio for Mac would then send back
an incorrect breakpoint line back to the .NET Core debugger, which again resulted in the debugger
sending back a corrected line. This would repeat resulting in the IDE and debugger
getting stuck in a loop.

**Fixed command line arguments not used with a new project**
    
Creating a new .NET Core console project, editing the project run
configuration to use extra command line arguments, or to not use the
external console, then building and running the project would result
in the project run configuration not being used. No extra
arguments would be passed to the console project, and the external
console would still be used. This could be fixed by closing and
re-opening the solution.
    
The problem was that when the project is re-evaluated, after it is
created, its run configurations are cleared. The solution's
startup run configuration would still be using
the original project run configuration that was no longer used.
Changes made to the project run configuration then had no affect. Closing and 
re-opening the solution fixed this since
the run configuration defined in the .csproj.user file is re-used when the
project is re-evaluated on reloading so both the solution run configuration and
the project run configuration refer to the same configuration. To fix this,
on re-evaluating the project, if the solution's run configuration
refers to a project run configuration that has been removed then
the solution's startup configuration is refreshed.
    
Note that there is a similar problem with multiple solution run
configurations that can occur which is not addressed by this fix.

**Fixed hang when discovering unit tests**
    
Opening a solution containing a .NET Core test project would sometimes
result in the IDE hanging when discovering tests. On running `kill -QUIT pid`
the IDE log would show a background thread and the UI thread both
awaiting test discovery to complete:
    
    var discoveredTests = await VsTestDiscoveryAdapter.Instance.DiscoverTestsAsync (Project);
    
    VsTestProjectTestSuite/<OnCreateTests>d__12.MoveNext
    in MonoDevelop.UnitTesting.VsTest/VsTestProjectTestSuite.cs:95

**Allow loading of SDK style projects without a main PropertyGroup**

On loading an SDK style project that did not have a main PropertyGroup
Visual Studio for Mac would show the error message 
"Error while trying to load project: 
Object reference not set to an instance of an object".

A project may define MSBuild properties in a Directory.Build.props file
instead of having this in the main project file. It is then
possible for the main project file to have no main property group.

Directory.Build.props:

    <Project>
      <PropertyGroup>
        <OutputType>Exe</OutputType>
        <TargetFramework>netcoreapp2.1</TargetFramework>
      </PropertyGroup>
    </Project>

MainProject.csproj:

    <Project Sdk="Microsoft.NET.Sdk">
    </Project>
    
Visual Studio for Mac now handles the missing main PropertyGroup.

**Fixed TargetFramework not being updated in project**
    
Changing a .NET Core project's target framework to a different version in
Project Options, then re-opening Project Options and changing the
target framework version back again, would result in the target framework
not being updated in the project. The problem was the original target framework the
project had on opening was cached and the changing back to the same
target framework version was being ignored resulting in the project file not
being updated.

**Fixed .xaml.cs code completion in new Xamarin.Forms .NET Standard project**
    
Creating a new Xamarin.Forms .NET Standard project, then
modifing the .xaml to add new named UI items, would result in no
code completion in the .xaml.cs file for these new items
until the solution was closed and re-opened. The problem was that the
.xaml and .xaml.cs files
were being removed from the file information held in memory
when the project was re-evaluated. On re-evaluation, after
the NuGet restore is first run for the project, the old MSBuild items for the
.xaml and .xaml.cs file have the wrong metadata, so they need
to be removed, whilst new MSBuild items with the updated metadata
need to be added. The removal was done after adding the updated files
and, since they had the same filename, the new updated files were being
removed. The removal is now done before adding the updated files to
avoid the files being removed incorrectly.

**Fixed default build action for new HTML file**
    
Adding a new .html file to the wwwroot folder of an ASP.NET Core
project would add the file as a None item instead of a Content
item. This would result in the .html file not being used when
publishing the project. When 'dotnet publish' was used the
publish directory would not contain the .html file.

ASP.NET Core projects have different build actions for files based on where they
are added. A .html file in the root directory would be a None item by default,
whilst a .html file in the wwwroot directory would be a Content item
by default. To fix this the default build action for a file is
determined by the file wildcard information available from the .NET
Core SDK.

**Fixed SDK version not being parsed from Sdk attribute**

The Sdk attribute would have its forward slash / replaced with a
backslash \ which meant Visual Studio for Mac was creating an
SdkReference with the wrong name, for example:
    
    Microsoft.NET.Sdk.Razor\2.1.0-preview2-final
    
Instead of having Microsoft.NET.Sdk.Razor as the name with the
version being separate.

**Fixed null reference on opening .NET Core project with SDK version**
    
Opening a SDK style project that used a Sdk attribute with a version
would show an error message "Error while trying to load project:
Object reference not set to an instance of an object". The problem
was that an SdkResolver can return null from its Resolve method.
These null results were added to a list and then an attempt was made
to log the result warnings on a null result.
    
    <Project Sdk="Microsoft.NET.Sdk.Razor/2.1.0-preview2-final">

**Fixed editor errors when .NET Standard assembly referenced in Xamarin.iOS project**
    
When a Xamarin.iOS project used an assembly that was compiled
for .NET Standard, such as the assembly in the
System.Collections.Immutable NuGet package, the netstandard assembly
was not made available for code completion. This then resulted in the text editor
showing errors even though the project could be compiled succesfully.
The errors displayed were similar to:
    
      The type 'ValueType' is defined in an assembly that is not
      referenced. You must add a reference to assembly 'netstandard,
      Version=2.0.0.0, Culture=neutral, PublicKeytoken=cc7b1dffcd2ddd51'.
    
Now a check is made to determine
if an assembly is referencing netstandard and if so the facade assemblies,
which for Xamarin.iOS will include the netstandard.dll, are made available
for code completion. Previously only a check was made for the project having
an assembly referencing System.Runtime before including the facade assemblies.