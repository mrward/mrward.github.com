---
layout: post
title: ".NET Core Support in Visual Studio for Mac 7.1"
date: 2017-08-21 21:00
comments: true
categories: NETCore Xamarin VSMac MonoDevelop
---

## New Features

   * .NET Core 2.0 support
   * New project templates
   * Target framework selection for new projects
   * Installed .NET Core Runtimes and SDKs shown in About Dialog
   * Improved support for multi target framework projects

More information on all the new features and changes in [Visual Studio for Mac 7.1](https://www.visualstudio.com/vs/visual-studio-mac/) can be found in the [release notes](https://www.visualstudio.com/en-us/news/releasenotes/vs2017-mac-relnotes#release-date-august-14-2017---visual-studio-for-mac-7101297).

## .NET Core 2.0 Support

Visual Studio for Mac 7.1 supports the [final release of .NET Core 2.0](https://blogs.msdn.microsoft.com/dotnet/2017/08/14/announcing-net-core-2-0/). The .NET Core 2.0 SDK needs to be [downloaded](https://www.microsoft.com/net/download/core) and installed separately.

For a .NET Core project the project options now lists the installed
runtimes available in the target framework list. For .NET Standard
projects if the 1.x runtimes are installed then the .NET Standard
1.0 through to 1.6 is shown as available in the target framework
list. If the .NET Core 2.0 runtime is installed then .NET Standard
2.0 will be shown in the target framework list.

If the project's target framework is not an installed runtime then
in the project options the target framework will indicate that
it is not installed.

If .NET Core 2.0 is installed then a .NET Core project will allow the selection of .NET Core 2.0 in the project options.

For a .NET Standard project you can select .NET Standard 2.0 if .NET Core 2.0 is installed.

## New Project Templates

The following new project templates have been added:

 - ASP.NET Core Web App (Razor Pages)
   - Only available if .NET Core 2.0 SDK is installed.
 - Class Library
   - This targets .NET Core instead of .NET Standard.
 - MSTest

A class library project has been added to the .NET Core - Library category
in the New Project dialog. This creates a class library project that
targets netcoreapp instead of netstandard. The main use for template would be
for creating a class library for ASP.NET Core projects where ASP.NET Core
NuGet packages are being used that do not support .NETStandard.

Some project templates do not support all the target framework versions. For example, the ASP.NET Core Web App (Razor Pages) only supports .NET Core 2.0, so this template will not be displayed if .NET Core 2.0 SDK is not installed.

Also the F# project templates have some restrictions on what target frameworks they support. The F# .NET Standard project templates, for example, do not support selecting .NET Standard versions below 1.6 when they are being created.

## Target Framework Selection for New Projects

If a project template can target multiple frameworks then a wizard page
will be displayed for that template allow one to be selected.

{% img /images/blog/DotNetCoreSupportInVisualStudioMac7-1/NewNetCoreProjectTargetFrameworkSelection.png 'New .NET Core target framework selection' 'New .NET Core target framework selection' %}

The framework selected will affect the target framework used by the project
but will also change which template is used. The ASP.NET Core 2.0 project
templates, for example, will typically use different NuGet package versions
compared with the ASP.NET Core 1.0 project templates.

The project template wizard will be displayed for all .NET Core
project templates allowing the target framework to be selected.
Projects that target .NET Core App can select from the installed
runtimes that are available. Projects that target .NET Standard can
select 2.0 if .NET Core 2.0 is installed, otherwise a 1.x version can be selected.
Some of the F# project templates do not support all target frameworks
so these are filtered from the list if an F# project is being created.

If only .NET Core SDK 2.0 is installed then the project templates will only support .NET Core 2.0, but will support .NET Standard 2.0 and 1.x.

If both .NET Core SDK 1.0 and 2.0 is installed then the project templates will support .NET Core 2.0 and 1.x, and .NET Standard 2.0 and 1.x

## Installed .NET Core Runtime and SDKs shown in About Dialog

The About dialog will now show the .NET Core runtimes and SDKs installed.

{% img /images/blog/DotNetCoreSupportInVisualStudioMac7-1/NetCoreInformationAboutDialog.png '.NET Core runtime and SDK information in About dialog' '.NET Core runtime and SDK information in About dialog' %}

## Improved Support for Multi Target Framework Projects

SDK style projects that target multiple frameworks can now be opened
and will show source files and NuGet package dependency information. Currently
the support is limited in comparison with Visual Studio on Windows.

The project will be treated as though it only has
one target framework which is the first one specified in the
TargetFrameworks property in the project file.

## Bug Fixes

**ASP.NET Core Web API project does not open API url on launching browser**

When an ASP.NET Core Web API project was created and then run it would open a blank web page in the browser instead of opening a page that showed the api values. Now when a new ASP.NET Core Web API project is created and run the url used when launching the browser will be http://localhost:<port>/api/values so the API values will be displayed.

**Unable to run or debug ASP.NET Core project**

If there was an empty directory inside the .NET Core SDK directory `/usr/local/share/dotnet/sdk` then it was
not possible to run or debug an ASP.NET Core web project

The problem was that if the Sdk imports were not found in the .NET
Core SDK then the OutputType was not read from the evaluated MSBuild
properties even though they were being set by the sdk imports that
are included with Mono. This bug could also occur if only the .NET
Core runtime is installed.

**Do not show files from shared project in Solution window**

When a .NET Core project referenced a shared assets project the files
from the shared project were being displayed in the Solution window
when they should not have been displayed. Visual Studio for Mac was
adding these imported files when the project was loaded.
Now the imported files are checked to see if they belong to a shared
project, indicated by the MSBuild project having the SharedGUID and
HasSharedItems properties, and if so they are ignored by the .NET
Core project extension, but will be added as Hidden, DontPersist
files by the shared assets project. This results in the files not
being displayed in the Solution window.

**Fix generated code for resx files in .NET Core projects**

Adding a resx file to a .NET Core 1.1 or 1.0 project would result in code
that could not be compiled.

Projects that target .NET Core App 1.x or .NET Standard 1.x cannot
compile code that uses "typeof(Resources).Assembly" which was
generated by the ResXFileCodeGenerator. Now if these target frameworks
are used by the project then the code generated uses
"typeof(Resources).GetTypeInfo().Assembly" which is supported.
.NET Core 2.0 and .NET Standard 2.0 do not require the use of
GetTypeInfo.

**Fix new resx file not added as Update item in Sdk projects**

Adding a new Resources file to a .NET Core project would add the
.resx file and the .Designer.cs file as an Include instead of an
Update item:

    <ItemGroup>
      <EmbeddedResource Include="Resources.resx">
        <Generator>ResXFileCodeGenerator</Generator>
        <LastGenOutput>Resources.Designer.cs</LastGenOutput>
      </EmbeddedResource>
    </ItemGroup>
    <ItemGroup>
      <Compile Include="Resources.Designer.cs">
        <DependentUpon>Resources.resx</DependentUpon>
      </Compile>
    </ItemGroup>

This then caused the build to fail since these files are part of
existing wildcard globs and should instead be Update items. This
problem affected new files that have extra metadata when they are
added to the project and then the project is saved.



