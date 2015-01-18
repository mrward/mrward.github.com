---
layout: post
title: "NuGet Support in Xamarin Studio 5.7"
date: 2015-01-18 12:00
comments: true
categories: NuGet Xamarin MonoDevelop
---

## Changes

   * NuGet menus renamed to make them easier to discover
   * Solution window icons made consistent
   * Fix build errors after MSBuild target restored for package
   * Fix types imported by MSBuild target not recognised after NuGet package installed
   * Fix Solution window cannot be opened when access to NuGet.Config is denied
   * Fix updating all packages not updating dependencies
   * Fix pre-release NuGet package being downgraded on update

More information on all the new features and changes in Xamarin Studio 5.7 can be found in the [release notes](http://developer.xamarin.com/releases/studio/xamarin.studio_5.7/xamarin.studio_5.7/).

## NuGet menus renamed

The menus have been changed so they now include the word NuGet to make it easier to find and use NuGet.

### Project menu

{% img /images/blog/NuGetSupportInXamarinStudio5-7/ProjectMenuNuGetMenuItems.png 'NuGet menu items in the main Project menu' 'NuGet menu items in the main Project menu' '' %}

### Solution context menu
    
{% img /images/blog/NuGetSupportInXamarinStudio5-7/SolutionContextMenuNuGetMenuItems.png 'NuGet menu items in the Solution context menu' 'NuGet menu items in the Solution context menu' '' %}

### Project context menu

{% img /images/blog/NuGetSupportInXamarinStudio5-7/ProjectContextMenuNuGetMenuItems.png 'NuGet menu items in the Project context menu' 'NuGet menu items in the Project context menu' '' %}

## Solution Window UI Changes

The error and warning icons used in the Solution window have been changed so they are consistent with the rest of icons used.

### Package not restored

{% img /images/blog/NuGetSupportInXamarinStudio5-7/SolutionWindowNuGetPackageMissing.png 'Solution Window - NuGet package not restored' 'Solution Window - NuGet package not restored' %}

{% img /images/blog/NuGetSupportInXamarinStudio5-7/SolutionWindowNuGetPackageMissingWithTooltip.png 'Solution Window - NuGet package not restored with tooltip' 'Solution Window - NuGet package not restored with tooltip' %}

A new warning icon is used for packages that are not restored, the text is greyed out and hovering over the warning icon shows a message that the package is not restored.

### Package installing

{% img /images/blog/NuGetSupportInXamarinStudio5-7/SolutionWindowNuGetPackageInstalling.png 'Solution Window - NuGet package installing' 'Solution Window - NuGet package installing' %}

The text is greyed out to indicate that the package is not currently available in the project and the text shows (installing) to distinguish between an unrestored package.

### Package needs retargeting

{% img /images/blog/NuGetSupportInXamarinStudio5-7/SolutionWindowNuGetPackageNeedsRetargetingWithTooltip.png 'Solution Window - NuGet package needs retargeting' 'Solution Window - NuGet package needs retargeting' %}

A new warning icon is used for packages that need retargeting. The package id text has changed to black text instead of orange. Hovering over warning icon shows a message that the package needs retargeting.

## Bug Fixes

**Build errors after MSBuild target restored for package**

If a NuGet package had an MSBuild target that added extra references to the project then on restoring the
NuGet package those references were still unavailable and the build would still fail.

This problem occurs with the MonoGame.Binaries NuGet package. The MonoGame.Binaries NuGet package has a custom MSBuild .targets file that adds extra references:

	<Project ToolsVersion="4.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
	  <ItemGroup>
	    <Reference Include="MonoGame.Framework">
	      <HintPath>$(MSBuildThisFileDirectory)\MonoGame.Framework.dll</HintPath>
	    </Reference>
	    <Reference Include="Tao.Sdl">
	      <HintPath>$(MSBuildThisFileDirectory)\Tao.Sdl.dll</HintPath>
	    </Reference>
	    <Reference Include="OpenTK">
	      <HintPath>$(MSBuildThisFileDirectory)\OpenTK.dll</HintPath>
	    </Reference>
	  </ItemGroup>
	</Project>

If the MonoGame.Binaries NuGet package is not available on opening the
project in Xamarin Studio the project will fail to build after
restoring the NuGet package since the references in the MSBuild targets file were not being refreshed.

Now after a NuGet package restore the MSBuild host used by Xamarin Studio is
stopped and restarted which allows the references in the MSBuild targets file to be found and the project to compile without any build errors.

**Types imported by MSBuild target not recognised after NuGet package installed**

If a NuGet package had an MSBuild target that added extra references to the project then on installing the
NuGet package the types from those references were still unavailable to Xamarin Studio and so the types would be highlighted in red
in the text editor. To fix this the solution had to be closed and re-opened. This problem occurs with the the MonoGame.Binaries NuGet package.

Now after a NuGet package is installed and it contains MSBuild targets
file then the type system will be refreshed for that project. The
types will then be known by Xamarin Studio and no longer show as red
text in the text editor.

**Solution window cannot be opened when access to NuGet.Config is denied**

Handling failure to read NuGet.Config

If the NuGet directory containing the NuGet.Config file can be created or read by NuGet then an
unhandled UnauthorizedAccessException is thrown. This exception was not being handled by Xamarin Studio and would prevent the
solution window from opening. Now if there is any error creating this
directory or trying to load the NuGet.Config file then the error is now caught and logged which allows the Solution window to open. If the NuGet directory containing the NuGet.Config file cannot be created then it will not be possible to use NuGet in the
Xamarin Studio but it will not prevent the solution pad from being used.

**Updating all packages not updating dependencies**

Updating NuGet packages for new Xamarin.Forms project would not install the Xamarin.Android.Support.v13 NuGet package.

The existing Xamarin.Forms project template used an older version of
Xamarin.Android.Support.v4 which does not depend on
Xamarin.Android.Support.v13. The latest Xamarin.Android.Support.v4
has a dependency on Xamarin.Android.Support.v13. When updating the
Xamarin.Android.Support.v4 NuGet package installed by the existing
Xamarin.Forms project template the Xamarin.Android.Support.v13 NuGet
package was not installed in a project that targets MonoAndroid 3.2
when all the packages in the solution were updated (right click
solution and select Update NuGet Packages). If the
Xamarin.Android.Support.v4 NuGet package was updated itself (right
click the package and select Update) or if all the packages for the
project were updated (right click Packages folder and select Update)
then Xamarin.Android.Support.v13 NuGet package would be installed.

The problem was the NuGet package update was not set to update any
NuGet package dependencies when updating all packages in the solution.
Updating all packages in the project or the NuGet package individually
was set to update package dependencies.

**Pre-release NuGet package being downgraded on update**

When a pre-release NuGet package was installed that was newer than the latest stable NuGet package then updating the package would install the stable version even though it was a lower version. Now an explicit check is made to ensure that an older NuGet package is not being installed.



