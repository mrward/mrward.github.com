---
layout: post
title: ".NET Core Support in Visual Studio for Mac 7.4"
date: 2018-03-11 11:00
comments: true
categories: NETCore Xamarin VSMac MonoDevelop
---

## Changes

   * Solution window detects file system changes outside Visual Studio for Mac
   * Projects with unknown target frameworks can now be loaded
     * [MSBuild SDK Extras](https://oren.codes/2017/08/29/use-all-tfms-with-sdk-style-projects-in-visual-studio-for-mac/) is now supported without requiring an extra extension to be installed
     * Tizen.NET projects are now supported
   * Fixed xUnit test messages not being displayed
   * ASP.NET Core minified files are no longer formatted
   * Support target framework defined in another MSBuild property
   * Show SDK information before dependencies are restored
   * Fixed failure to add new file when the default project namespace contains only numbers
   * Fixed remove item not added for .xaml.cs file
   * Fixed incorrect project updates when moving a file

More information on all the new features and changes in [Visual Studio for Mac 7.4](https://www.visualstudio.com/vs/visual-studio-mac/) can be found in the [release notes](https://docs.microsoft.com/en-us/visualstudio/releasenotes/vs2017-mac-relnotes#15.6).

## Solution window detects file system changes

Files created in the project directory, or a subdirectory,
outside Visual Studio for Mac are now added to the project and
appear in the Solution window automatically.

{% img /images/blog/NetCoreSupportInVisualStudioMac7-4/DotNetCoreProjectFileWatcher.gif 'File added and removed from command line - Solution window updated' 'File added and removed from command line - Solution window updated' %}

Deleting project files and directories will remove the files from
the Solution window. The Solution window will also update after
a file or a directory is renamed outside Visual Studio for Mac.

**Allow unknown target framework projects to be loaded**

The target framework of an SDK style project is no longer restricted.
Previously Visual Studio for Mac would fail to load an SDK style
project unless its target framework was .NET Standard, .NET Core App
or .NET Framework.

This allows Tizen projects to be loaded without the error message
"Project does not support framework 'Tizen,Version=v4.0'" being
displayed. Tizen projects have a Tizen.NET package reference which
imports an MSBuild .props file that defines the Tizen target framework.
    
The [MSBuild.Sdk.Extras](https://github.com/onovotny/MSBuildSdkExtras) NuGet 
package is now supported so SDK style projects can use target frameworks
that this NuGet package
supports, such as Xamarin.iOS and MonoAndroid.
    
On building a project with an unknown target framework, that is 
not defined by any used NuGet package references, a build error 
that indicates the framework is not supported will occur:
    
    The TargetFramework value 'test1.0' was not recognized.
    
NuGet restore will also fail for an unknown target framework with
a message indicating the framework is invalid.

## Bug Fixes

**Fix xUnit test messages not displayed**
    
xUnit tests support an ITestOutputHelper interface which can be
passed to the constructor of the test. This has a WriteLine method
which can be used to output messages whilst running the test. When
this was used the text displayed in Visual Studio for Mac's Test Results
window was:
    
    Microsoft.VisualStudio.TestPlatform.ObjectModel.TestResultMessage
    
Now the text from the message is shown in the Tests Results window.

**Support target framework defined in another MSBuild property**
    
An SDK style project that defined the <TargetFrameworks> element to
be another MSBuild property would fail to restore.

In the example below the property TheFramework is defined in the
Frameworks.props file.
    
    <Project Sdk="Microsoft.NET.Sdk">
      <Import Project="Frameworks.props" />
      <PropertyGroup>
        <TargetFrameworks>$(TheFramework)</TargetFrameworks>
      </PropertyGroup>
    </Project>

The unevaluated MSBuild property value was used and would result in the
NuGet package restore failing.

Now the evaluated property value is used when restoring dependencies.

**ASP.NET Core minified files and map files are no longer formatted**

On creating a new ASP.NET Core project the minified files were being 
re-formatted and were no longer minified. Now the .min.css, .min.js and
.map files are excluded from being re-formatted when a new ASP.NET Core 
project is created.

**Fixed failure to add a new file when the default project namespace contains only numbers**
    
If a project has a default namespace of '1' then adding a new C#
file from a template would throw a null reference
exception. The problem was that the sanitized namespace for the project's
default namespace was null.

**Show SDK information before dependencies are restored**

When a new .NET Core project is created the Dependencies - SDK folder
would show no items until the NuGet package restore completed. Now 
the SDK dependency is shown whilst the dependencies are being restored.

**Fixed remove item not added for .xaml.cs file**
    
When a new content page with xaml file was
added and then removed, but not deleted, 
a remove item was not added for the .xaml.cs file.

**Fixed incorrect project updates when moving a file**
    
Moving a Resource file from one folder to another would not update
the project file correctly. Either the project file would not be
modified or the file would be removed.

One problem was that on moving a clone is taken of the original item
and the associated MSBuild item was copied but was still referring
back to the original location. Another problem was that an incorrect
match was being made when looking for remove items resulting in the
wrong files being removed from the project.