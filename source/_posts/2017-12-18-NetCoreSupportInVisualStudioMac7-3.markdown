---
layout: post
title: ".NET Core Support in Visual Studio for Mac 7.3"
date: 2017-12-18 12:00
comments: true
categories: NETCore Xamarin VSMac MonoDevelop
---

## Changes

   * [VSTest](https://github.com/Microsoft/vstest) Console Runner now included with Visual Studio for Mac
      * Allows [xUnit](https://xunit.github.io) tests to be supported for non .NET Core projects
   * Renamed ASP.NET Core Project Templates
   * New ASP.NET Core F# Project Templates Added
   * Support Backslash in Project Template Primary Output Path
   * Support [NuGet Restore MSBuild Properties](https://github.com/NuGet/Home/wiki/%5BSpec%5D-NuGet-settings-in-MSBuild)
     * RestoreAdditionalProjectFallbackFolders
     * RestoreAdditionalProjectSources
     * RestoreFallbackFolders
     * RestorePackagesPath
     * RestoreSources
   * Fixed transitive project references not supported

More information on all the new features and changes in [Visual Studio for Mac 7.3](https://www.visualstudio.com/vs/visual-studio-mac/) can be found in the [release notes](https://www.visualstudio.com/en-us/news/releasenotes/vs2017-mac-relnotes#release-date-december-4-2017---visual-studio-2017-version-73-730797)

## VSTest Console Runner

Previous versions of Visual Studio for Mac would use **dotnet vstest**,
included with the .NET Core SDK, 
to run unit tests for .NET Core projects. Now Visual Studio for Mac includes
the [VSTest](https://github.com/Microsoft/vstest) Console runner and uses that instead of using the .NET Core SDK.

The standalone VSTest Console runner allows other project types, not just .NET Core projects, to
be supported. Visual Studio for Mac can now run [xUnit](https://xunit.github.io) tests
for .NET Framework projects. Previously only NUnit was supported.

Let us take a look at using xUnit in a .NET Framework project.

First create a .NET Framework Library project by opening the New Project dialog and selecting
Library from the Other - .NET - General category.

Install the following NuGet packages into the project.

 - xunit
 - xunit.runner.visualstudio

Edit MyClass.cs and add some xUnit tests, as shown below:

```

using Xunit;

namespace xUnitTests
{
	public class MyClass
	{
		[Fact]
		public void PassingTest()
		{
			Assert.True(true);
		}

		[Fact]
		public void FailingTest()
		{
			Assert.False(true);
		}
	}
}

```

Building the project should then show the xUnit tests in the Unit Tests window.

{% img /images/blog/NetCoreSupportInVisualStudioMac7-3/xUnitTestsInUnitTestsWindow.png 'xUnit tests in Unit Tests window' 'xUnit tests in Unit Tests window' %}

Note that, due to a bug in Visual Studio for Mac, the tests may not appear in the
Unit Tests window if the
xunit.runner.visualstudio was not available in the local machine's NuGet
package cache when it was added to the project. Closing and re-opening the
solution should allow the tests to be discovered.

The tests can then be run or debugged in the usual way.

Currently MSTest is not yet supported in .NET Framework projects. The tests are discovered
but they fail to run with an access denied error message:

    An exception occurred while invoking executor 'executor://mstestadapter/v2': 
    Access to the path "/TestResults" is denied.

## Renamed ASP.NET Core Project Templates

The ASP.NET Core template names displayed in the New Project dialog
have been changed to be more consistent with Visual Studio on Windows.

 - 'ASP.NET Core Web App (Razor Pages)' renamed to 'ASP.NET Core Web App'
 - 'ASP.NET Core Web App' renamed to 'ASP.NET Core Web App (MVC)'

Visual Studio uses (Model-View-Controller) but the shorter
MVC is used by Visual Studio for Mac.

## New ASP.NET Core F# Project Templates Added

The following ASP.NET Core F# project templates are now available from the
New Project dialog.

 - ASP.NET Core Empty
   - .NET Core 1.x and 2.0
 - ASP.NET Core Web API
    - .NET Core 2.0 only

## Support Backslash in Project Template Primary Output Path

The template.json file used by the new templating engine uses a
primaryOutputs property for projects and files. If the paths used
contained backslashes then the project was not added to the
solution.

```
"primaryOutputs": [
  { "path": "Library1\\Library1.csproj" },
  { "path": "Library2\\Library2.csproj" }
]
```

Now the paths have the backslashes replaced with forward slashes if
the current platform does not support backslash as a path
separator.

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

**Fixed Transitive Project References not supported**

With three .NET Core projects, LibC referencing LibB referencing LibA,
the types defined by LibA were not available to LibC unless it was
directly referencing LibA. In Visual Studio for Mac 7.3 the types 
defined by LibA are available to LibC without LibC directly
referencing LibA.

**Portable Class Library projects can now be referenced by .NET Core projects**

Portable Class Library (PCL) projects could not be referenced by .NET Core
projects or .NET Standard projects. The Edit References dialog would show
"Incompatible target framework" for the PCL project when trying to add the
reference.

Visual Studio for Mac now has the same behaviour as Visual Studio on Windows where
a .NET Standard project can reference any PCL project.

**Fixed MSBuild Update item not removed from project**

If a file was deleted from a project, or removed without deleting,
and the file was the last file for a particular wildcard include, the
Update item for the file was not being removed from the project.

**Fixed MSBuild Remove item not added when Update item exists**

With a project that contained a single .cs file, and that file had
an Update item defined, when the file was removed from the project,
without deleting it, a Remove item was not being added to the project for the file.

**Fixed Remove item not being added when file excluded from project**

When the last file was removed from the project, but not deleted, an
MSBuild Remove item was not added to the project for the file.

 1. Create a .NET Standard 2.0 project.
 2. Right click Class1.cs and select Remove.
 3. Select Remove from Project in the dialog that opens.
 4. Close and re-open solution.

The Class1.cs file was still shown in the Solution window when it should have been
removed. The project file did not have a Compile remove item for Class1.cs when it should
have been added:

    <Compile Remove="Class1.cs" />

**Deleting a XAML file left an MSBuild Remove item in project file**

When a .xaml file in a project was deleted the associated
Remove MSBuild item, if it was present, was not being removed from the
project file.

  1. Create a new .NET Standard 2.0 project.
  2. Add Xamarin.Forms 2.3 NuGet package.
  3. Add a new Content Page with Xaml file using the New File dialog.
  4. In the Solution window select the .xaml file and delete it.

All MSBuild items associated with the .xaml file should have been
removed from the project file but instead a None item would remain.

    <None Remove="MyPage.xaml" />

**Fix MSBuild remove item not added when adding a XAML file**

Adding a new .xaml file to a project was not adding a None Remove
item for the .xaml file to remove it from the default wildcard include for all
files.

    <None Remove="MyNewPage.xaml" />

On reloading the solution the Solution window would show two .xaml
files - one EmbeddedResource item and one None item.

**Fixed MSBuild condition evaluation when using Czech locale**

With the primary operating system language set to Czech, creating a new .NET
Core project would result in the project having a target framework of net461.

The problem was that Single.TryParse was returing true for an empty string
when the machine's culture was 'cs-CZ'. This was causing the MSBuild condition
evaluation to be treated as a boolean comparison incorrectly in some cases.
The condition below would return false even though the property was
not defined and both sides of the condition were empty strings:

    <PropertyGroup Condition="'$(TargetFrameworkIdentifier)' == ''">

This was causing the TargetFrameworkIdentifier to not be set for
.NET Core projects so the default of .NETFramework was being used.

**Re-evaluate AppliesTo condition on capability change**

Fixed the AppliesTo attribute not attaching a project extension to
a project after a NuGet package restore enabled a project capability.
The AppliesTo attribute allows a project extension to be associated
to a project based on its capabilities.

If the project extension uses an AppliesTo attribute which is
initially false and then a new project capability is added, and the
project is re-evaluated, the AppliesTo attribute is now
re-evaluated. This will then cause the project extension to be
attached to the project. If the project extension was attached to the
project and the project capability is removed then the project
extension will now be removed when the project is re-evaluated.

**Fixed being unable to run a new Azure Functions project**

Creating a new Azure Functions project would result in
being unable to run the project until the solution was closed
and re-opened again. After the NuGet packages are restored the
project can be run but this information was not updated in Visual Studio
for Mac.

Now after a NuGet package restore, when re-evaluating the project,
the solution startup item is updated if there is no startup item
already defined.

When the project capability is removed, due to a NuGet package
being uninstalled, and the
project can no longer be run the startup item will be updated if
it was previously set to be this project.

**Fix generated NuGet files being imported twice**
    
The ProjectName.nuget.g.targets and ProjectName.nuget.g.props,
that are generated for .NET Core projects in the base intermediate
directory, were being imported twice.
Once by Microsoft.Common.props, that is provided with Mono, and once
again by Visual Studio for Mac.
    
Importing these files twice was causing a duplicate file to be added to
the project information held in memory by Visual Studio for Mac
when Xamarin.Forms 2.4 was used and no .NET Core SDK was installed. 
This would result in the content page xaml and associated C# file not 
being nested in the solution window.

**Fixed items added to project when a new content page with XAML was created**

Adding a new Xamarin.Forms content page with XAML would add an
update item for the .xaml.cs file, a remove item for the xaml file,
and an include item for the xaml file.
    
An example is Xamarin.Forms 2.4 which defines wildcard includes:
    
    <None Remove="**\*.xaml" />
    <EmbeddedResource Include="**\*.xaml" SubType="Designer" Generator="MSBuild:UpdateDesignTimeXaml" />
    
Which would cause a new .xaml file added to the project and saved
in the project file as well as a remove for the None item even though
the .xaml file was already removed from the None items.
    
    <None Remove="MyView.xaml" />
    
    <EmbeddedResource Include="MyView.xaml">
      <Generator>MSBuild:UpdateDesignTimeXaml"</Generator>
    </EmbeddedResource>

**Fix DependentUpon property being saved in project**
    
Xamarin.Forms 2.4 defines an update item similar to:
    
    <Compile Update="**\*.xaml.cs" DependentUpon="%(Filename)" />
    
When a project was saved a Compile item was added to the main project
with the filename stored in the DependentUpon element. Now
the evaluated DependentUpon value is treated as being the default 
value so it is not added to the project file.

**Fix xaml.cs files not being nested**
    
Xamarin.Forms 2.4 defines an update item similar to:
    
    <Compile Update="**\*.xaml.cs" DependentUpon="%(Filename)" />
    
The DependentUpon property was being evaluated as '*.xaml.cs'. 
This was causing .xaml.cs files to not be nested in the Solution window.

**Fixed build error when Xamarin.Forms 2.4 used in a .NET Standard project**

If a Xamarin.Forms .NET Standard project had a
.xaml file and a .xaml.cs file then the xaml.g.cs file would not be
generated when the project file contained no files. The
default items defined by the Xamarin.Forms NuGet package were not 
being imported. This then caused a build error about the
InitializeComponent method not being defined.

Xamarin.Forms 2.4.0 uses default
item imports which were not being included since they are conditionally
imported and the MSBuildSDKsPath was not defined:
    
    <Import Project="$(MSBuildThisFileDirectory)Xamarin.Forms.DefaultItems.props" Condition="'$(MSBuildSDKsPath)'!=''" />

MSBuild on the command line defines the MSBuildSDKsPath globally in
its MSBuild.dll.config file. The MSBuild engine host, used when
building a project in Visual Studio for Mac, now also defines the 
MSBuildSDKsPath property.

**Fixed remove item added when removing a linked file**
    
Deleting a file link in a .NET Core project would add a remove item
for the file instead of removing the link from the project.
