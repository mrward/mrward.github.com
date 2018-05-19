---
layout: post
title: "NuGet Support in Visual Studio for Mac 7.5"
date: 2018-05-19 15:30
comments: true
categories: NuGet Xamarin VSMac MonoDevelop
---

## Changes

   * Support installing NuGet packages with item templates
   * Fixed generated NuGet package files not available for code completion
   * Fixed incorrect Android target framework used when the project has Package References
   * Fixed build errors after creating a new Android project with NuGet packages

More information on all the new features and changes in [Visual Studio for Mac 7.5](https://www.visualstudio.com/vs/visual-studio-mac/)
can be found in the [release notes](https://docs.microsoft.com/en-us/visualstudio/releasenotes/vs2017-mac-relnotes#-visual-studio-2017-for-mac-version-75-release-notes).
    
# Support installing NuGet packages with item templates

Initial support for creating new files from item templates that use 
the new dotnet templating engine has been added. This is currently used
by the Azure Functions support in Visual Studio for Mac when a new Azure
Function is added to the project. Currently the New File dialog does not
have support for showing these templates but there is an API that can
be used by extensions to create files from these templates.

The item templates can define post actions that indicate that a NuGet
package is needed. These post actions are read and the NuGet
package will be installed into the project by Visual Studio for Mac.

    "postActions": [
      {
        "Description": "Adding Reference to Microsoft.Azure.WebJobs.Extensions.DocumentDB Nuget package",
        "ActionId": "B17581D1-C5C9-4489-8F0A-004BE667B814",
        "ContinueOnError": "true",
        "ManualInstructions": [],
        "args": {
          "referenceType": "package",
          "reference": "Microsoft.Azure.WebJobs.Extensions.DocumentDB",
          "version": "1.2.0",
          "projectFileExtensions": ".csproj"
        }
      }

## Bug Fixes

**Generated NuGet package files not available for code completion**
    
On installing a NuGet package such as Refit, which extends the
CoreCompileDependsOn to generate a C# file, the generated file would
not be available for code completion until the solution was closed
and re-opened again.

Below is a section of the refit.targets file used by the Refit NuGet package.

    <PropertyGroup>
      <CoreCompileDependsOn>
        $(CoreCompileDependsOn);
        GenerateRefitStubs;
      </CoreCompileDependsOn>
    </PropertyGroup>

    <Target Name="GenerateRefitStubs" BeforeTargets="CoreCompile">

      <Error Condition="'$(MSBuildRuntimeType)' == 'Core' and '$(RefitMinCoreVersionRequired)' > '$(RefitNetCoreAppVersion)' "
           Text="Refit requires at least the .NET Core SDK v2.0 to run with 'dotnet build'"
           ContinueOnError="false"
           />
    
      <GenerateStubsTask SourceFiles="@(Compile)" BaseDirectory="$(MSBuildProjectDirectory)"   OutputFile="$(IntermediateOutputPath)\RefitStubs.g.cs" />

      <Message Text="Processed Refit Stubs" />      
   
      <ItemGroup Condition="Exists('$(IntermediateOutputPath)\RefitStubs.g.cs')">
        <Compile Include="$(IntermediateOutputPath)\RefitStubs.g.cs" />
      </ItemGroup>    
    </Target>

The CoreCompileDependsOn property is re-defined so the GenerateRefitsStubs MSBuild
target will be run. This creates a new RefitStubs.g.cs file which is included in the
project by the Compile MSBuild item.

The problem was that the result of running the
targets that are defined by the CoreCompileDependsOn property are cached by Visual
Studio for Mac.
When Refit is installed the cached result was still being returned so the
generated C# file was not available for code completion. To fix this the 
project is now re-evaluated before getting the CoreCompileDependsOn property 
if an MSBuild import has been added or removed. Note that projects that use
Package References were unaffected. Only projects that use a packages.config 
file were affected.

**Incorrect Android target framework used when the project has Package References**

Configuring an Android project so it uses the latest Android framework
could result in the wrong target framework being used when building if the
project used Package References. This would result in a build error
similar to:

    Error: Your project is not referencing the "MonoAndroid,Version=v7.1" framework. 
    Add a reference to "MonoAndroid,Version=v7.1" in the "frameworks" section of your 
    project.json, and then re-run NuGet restore

The problem was that the wrong target framework was being used in the
project.assets.json. Running a NuGet restore was
a workaround for the problem.

Now when the target framework version in project options is changed, and
the project uses Package References, a NuGet restore is run after the
project file is saved. This ensures the Package References work with
the new target framework. This also re-generates the project.assets.json file
so the build will use the correct references.

Another problem here was that configuring the Android to use the latest
target framework would not result in the target framework being changed
in the project model stored in memory.
This could also result in the project.assets.json file being out of
sync with the Android project's target framework.

**Build errors after creating a new Android project with NuGet packages**

Creating a new Android project that installed NuGet packages as it was created
would sometimes result in build errors that could be fixed by closing and
re-opening the solution. An example build error is shown below.

```
    Resources/values/styles.xml : error APT0000: 1: error: Error retrieving parent for item: No resource found that matches the given name 'Theme.AppCompat.Light.DarkActionBar'.
    Resources/values/styles.xml : error APT0000: 2: error: Error: No resource found that matches the given   name: attr 'colorAccent'.
    Resources/values/styles.xml : error APT0000: 1: error: Error: No resource found that matches the given name: attr 'colorPrimary'.
    Resources/values/styles.xml : error APT0000: 1: error: Error: No resource found that matches the given name: attr 'colorPrimaryDark'.
    Resources/values/styles.xml : error APT0000: 1: error: Error: No resource found that matches the given name: attr 'windowActionBar'.
    Resources/values/styles.xml : error APT0000: 3: error: Error: No resource found that matches the given name: attr 'windowActionModeOverlay'.
    Resources/values/styles.xml : error APT0000: 1: error: Error: No resource found that matches the given name: attr 'windowNoTitle'.
    Resources/values/styles.xml : error APT0000: 3: error: Error retrieving parent for item: No resource found that matches the given name 'Theme.AppCompat.Light.Dialog'.
    Resources/values/styles.xml : error APT0000: 3: error: Error: No resource found that matches the given name: attr 'colorAccent'.

The build errors varied but in each case the error was caused by a
missing reference to an assembly from one of the Android Support NuGet packages installed
when the project was created. The project file created would have
the correct reference information but Visual Studio for Mac was
building an old version of the project.
    
If the project was modified in memory when a build is requested this
results in the unsaved project xml being given to the remote MSBuild
host. In some cases this project xml was not being cleared and would 
result in an out of date project xml being used when building. This 
could occur if the MSBuild host did not have any projects 
loaded at the time when an attempt was made to unload the project from 
the host. Now the unsaved project xml is removed in all cases when the 
project is unloaded from the MSBuild host.

**Fix missing child package dependencies in Solution window**

After creating a new ASP.NET Core project sometimes the package dependencies
shown in the Solution window under the SDK folder and the NuGet folder 
could not be expanded to view the child dependencies.

The problem was that if the call to get the package dependencies
using MSBuild was cancelled this would result an empty list of depenencies
being cached. Since no package dependencies were returned the Solution window
would fallback to showing the package dependencies that were listed in the
project and a default top level SDK package dependency.