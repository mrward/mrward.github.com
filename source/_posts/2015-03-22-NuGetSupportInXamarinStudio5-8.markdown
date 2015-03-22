---
layout: post
title: "NuGet Support in Xamarin Studio 5.8"
date: 2015-03-21 12:00
comments: true
categories: NuGet Xamarin MonoDevelop
---

Xamarin Studio 5.8 [added support for Apple's Watch Kit](http://blog.xamarin.com/xamarin-releases-watch-kit-support-like-clockwork/) and it also includes some NuGet bug fixes.

## Bug Fixes

   * Allow ASP.NET project templates to work offline.
   * Build error after updating Xamarin.Forms in a project created by Visual Studio
   * Custom MSBuild task not updated after updating Xamarin.Forms
   * Update information in the Solution window incorrect after updating packages
   * Check for package updates continues after closing a solution
   * Check for package updates prevents packages from being removed
   * Incorrect error message displayed when checking for package updates
   * Exception when checking for package updates after changing target framework of a project

More information on all the new features and changes in Xamarin Studio 5.8 can be found in the [release notes](http://developer.xamarin.com/releases/studio/xamarin.studio_5.8/xamarin.studio_5.8/).

**Allow ASP.NET project templates to work offline**

Previously it was not possible to create an ASP.NET project without an internet connection even if all the NuGet packages were available in the local machine's NuGet packages cache. 

Now the local machine's NuGet cache is used as the primary source of packages for project templates so it is possible to create an ASP.NET project without an internet connection if the NuGet packages are already in this cache.

This problem did not affect project templates, such as Xamarin.Forms, which include the NuGet packages with their project templates.

**Build error after updating Xamarin.Forms in a project created by Visual Studio**

When a NuGet package that uses custom MSBuild targets file, such as Xamarin.Forms, is added to a project by Visual Studio the project file has an extra Target added, as shown below.

    <Target Name="EnsureNuGetPackageBuildImports" BeforeTargets="PrepareForBuild">
      <PropertyGroup>
        <ErrorText>This project references NuGet package(s) that are missing on this computer.
    Enable NuGet Package Restore to download them.  For more information, see
    http://go.microsoft.com/fwlink/?LinkID=322105. The missing file is {0}.</ErrorText>
      </PropertyGroup>
      <Error Condition="!Exists('packages\Xamarin.Forms.1.2.1.6229\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10\Xamarin.Forms.targets')" Text="$([System.String]::Format('$(ErrorText)', 'packages\Xamarin.Forms.1.2.1.6229\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10\Xamarin.Forms.targets'))" />
    </Target>

When the project was opened in a previous version of Xamarin Studio, and the NuGet package is updated or removed, the Error task was not updated. The project would then fail to compile with an error being reported that the old .targets file could not be found.

Now when updating or removing a NuGet package the EnsureNuGetPackageBuildImports target is
checked and the matching Error task will be removed. If there are no remaining Error tasks then the EnsureNuGetPackageBuildImports target is also removed. This prevents build errors after updating or uninstalling the old
NuGet package from the project.

Note that Xamarin Studio will not add an EnsureNuGetPackageBuildImports target and will not add Error tasks to a project when a NuGet package is added or updated.

**Custom MSBuild task not updated after updating Xamarin.Forms**

If a project that had a reference to Xamarin.Forms was compiled once, then the NuGet package was updated, the old MSBuild task was still being used when compiling.

For Xamarin.Forms this could cause a "XamlG Task failed unexpectedly" build error to be reported. Also the Package Console would report not being able to access the Xamarin.Forms.Build.Tasks.dll when updating or removing the NuGet package.

The problem was that MonoDevelop.Projects.Formats.MSBuild.exe that compiles the project would lock the MSBuild task assembly.

Now when an MSBuild import is removed on updating a NuGet package Xamarin Studio will dispose the current project builder which will shutdown the MonoDevelop.Projects.Formats.MSBuild.exe process. This unlocks any custom MSBuild task assemblies loaded by this process, 
allowing the old NuGet package to be removed without any access denied errors, and when the project is recompiled again it will use the correct MSBuild task assembly.

**Update information in the Solution window incorrect after updating packages**

With check for updates enabled in Preferences, when an ASP.NET project is created, updates are shown as available for several packages. When
the packages are updated the Solution window would still show updates as being available even though the packages had been updated.

The problem was that a change was made in Xamarin Studio 5.7 to update package dependencies at the same time as the package was updated. This meant that an update event was not fired for each
package being updated which would result in some package updates being shown as available in the Solution window even when they had been updated.

Now Xamarin Studio will check all package references when a package is updated so if any package dependencies are updated the Solution
window will show the correct status of the packages.

**Check for package updates continues after closing a solution**

Xamarin Studio will now stop checking for package updates when the current solution is closed. Previously this would continue until the check was completed.

**Check for updates prevents packages from being removed**

Previously when Xamarin Studio was checking for package updates all other NuGet actions, such as updating, adding or removing packages, would not be run until the check for updates had completed.
Now the check for NuGet package updates is done on a separate thread so other NuGet actions can be run at the same time.

**Incorrect error message displayed when checking for package updates**

The Package Console would sometimes show the error message "An exception was thrown while dispatching a method call in the UI thread." instead of the underlying error making it difficult to determine the cause of the problem.
For example if there was a zero byte sized .nupkg file in the solution's package directory, and check for updates is enabled, then 
the wrong error was displayed in the Package Console.

**Exception when checking for package updates after changing target framework of a project**

Previously a null reference exception would be reported in the Package console window when doing the following:

  1. Create a project with one NuGet package added.
  2. Create two NuGet package sources in Preferences. Disable all other package sources.
  3. Open the Add Packages dialog and select All Sources.
  4. Go back to Preferences and uncheck both of the package sources.
  5. Change the target framework of the project in the project options.
