---
layout: post
title: "NuGet Support in Xamarin Studio 5.8"
date: 2015-03-21 12:00
comments: true
categories: NuGet Xamarin MonoDevelop
---

Whilst Xamarin Studio 5.8 main focus was to [add support for Apple's Watch Kit](http://blog.xamarin.com/xamarin-releases-watch-kit-support-like-clockwork/) it also includes some bug fixes for the NuGet addin.

## Changes

   * Allow ASP.NET project templates to work offline.
   * Fix build error after updating Xamarin.Forms NuGet package in a project created by Visual Studio
   * Fix exception when checking for NuGet package updates after changing target framework of a project
   * Fix Custom MSBuild task not updated after updating NuGet package for Xamarin.Forms
   * Fix updating all packages in a solution does not refresh the update information in the Solution window
   * Fix checking for package updates continues after closing/switching solutions
   * Fix check for updates updates prevents packages from being removed
   * Fix incorrect error message displayed when checking for package updates

More information on all the new features and changes in Xamarin Studio 5.8 can be found in the [release notes](http://developer.xamarin.com/releases/studio/xamarin.studio_5.8/xamarin.studio_5.8/).

## Allow ASP.NET project templates to work offline.

With all the NuGet packages needed for the ASP.NET project templates available in the local machine's cache it was not possible to create a new ASP.NET project without the NuGet packages failing to install. This problem does not affect project templates, such as Xamarin.Forms, which includes the NuGet packages alongside its project templates.
Now the local machine's NuGet cache is used as the primary source of packages for project templates so it is possible to create an ASP.NET project without an internet connection if the NuGet packages are in the cache.

## Bug Fixes

**Build error after updating Xamarin.Forms NuGet package in a project created by Visual Studio**

When adding a NuGet package that uses custom MSBuild targets file to a project in Visual Studio the project file has an extra Target added that checks that the MSBuild targets file exists.

  <Target Name="EnsureNuGetPackageBuildImports" BeforeTargets="PrepareForBuild">
    <PropertyGroup>
      <ErrorText>This project references NuGet package(s) that are missing on this computer.
Enable NuGet Package Restore to download them.  For more information, see
http://go.microsoft.com/fwlink/?LinkID=322105. The missing file is {0}.</ErrorText>
    </PropertyGroup>
    <Error Condition="!Exists('packages\Xamarin.Forms.1.2.1.6229\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10\Xamarin.Forms.targets')" Text="$([System.String]::Format('$(ErrorText)', 'packages\Xamarin.Forms.1.2.1.6229\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10\Xamarin.Forms.targets'))" />
  </Target>

When the project was opened in an older version of Xamarin Studio, and the NuGet package is updated or removed, the Error task was not updated. When the project was then compiled the build would fail with an error being reported that the old .targets file could not be found.

Now when updating or uninstalling a NuGet package that uses an MSBuild .targets file the EnsureNuGetPackageBuildImports target is
checked and if there is a matching Error task for that .targets file the Error task is removed. If there are no remaining Error tasks after
this removal then the EnsureNuGetPackageBuildImports target is removed too. If there are other Error tasks remaining then the
EnsureNuGetPackageBuildImports target is left in the project file. This prevents build errors after updating or uninstalling the old
NuGet package from the project. Note that Xamarin Studio will not add an EnsureNuGetPackageBuildImports target when
adding a NuGet package that uses an MSBuild .targets file and will not add Error tasks to this target.

**Exception when checking for NuGet package updates after changing target framework of a project**

Previously a null reference exception would be reported in the Package console window when doing the following:

1) Create a project with one NuGet package added.
2) Create two NuGet package sources in Preferences. Check both these
two sources so they are both enabled. All the rest should be disabled.
3) Open the Add Packages dialog and select select All Sources.
4) Go back to Preferences and uncheck both of the two package sources.
5) Change the target framework of the project in Build/General.


**Custom MSBuild task not updated after updating NuGet package for Xamarin.Forms**

If a project that had a reference to Xamarin.Forms was built once, and then the NuGet package was updated, the old MSBuild task dll was still being used when compiling after the update.

To reproduce this bug:

1) Create a Xamarin.Forms project and install Xamarin.Forms.1.0.6186
2) In the PCL project add a new content page so the Xaml Generator is called on building the project.
3) Rebuild the PCL project. Compiling should fail with an error:
"XamlG Task failed unexpectedly"
4) Update the Xamarin.Forms NuGet package in the PCL project to the latest version.
5) Rebuild the PCL project again. The "XamlG Task failed unexpectedly" error is shown again. Also you can see in the Package Console that the
NuGet addin could not access the Xamarin.Forms.Build.Tasks.dll since the Package Console reports the warning: "Access to the path
'Xamarin.Forms.Build.Tasks.dll' is denied."

The problem is that MonoDevelop.Projects.Formats.MSBuild.exe that compiles the project has locked the MSBuild task dll.

Now when an MSBuild import is removed on updating a NuGet package the NuGet addin will dispose the current project builder for the project
which will kill the MonoDevelop.Projects.Formats.MSBuild.exe process. This unlocks any custom MSBuild task dlls locked by the process and
allows the old NuGet package to be removed without any access denied errors and also means when the project is recompiled again after the
update it will use the correct MSBuild task dll.

**Updating all packages in a solution does not refresh the update information in the Solution window**

With check for updates enabled in Preferences, when an ASP.NET project is created, updates are shown as available for several packages. When
the packages are updated for the solution or for the project the Solution window still shows updates as being available even though the packages have been updated.

The problem was that a change was made to the NuGet addin to update a package dependencies at the same time as the package, also an extra
check is made to ensure the update is not trying to downgrade the NuGet package. This meant that an update event was not fired for each
package being updated which results in some package updates being shown as available in the Solution window even when they have been updated.

Now the NuGet addin will check all package references when a package is updated so if any package dependencies are updated the Solution
window will be updated to reflect the current status of the packages.

**Check for package updates continues after closing/switching solutions**

Xamarin Studio will now stop checking for package updates when the current solution is closed.

**Check for updates updates prevents packages from being removed**

Checking for NuGet package updates is no longer done on the dispatcher background thread.

Checking for NuGet package updates will also be stopped if the solution is closed.

**Incorrect error message displayed when checking for package updates**

If there is a zero byte .nupkg file in the solution's package directory and check for updates is enabled then the error message
shown in the Package Console was "An exception was thrown while dispatching a method call in the UI thread."

The DispatchService wraps the exception with its own so if a call is dispatched to the UI thread the underlying error message is lost if
you use ex.Message and not ex.ToString (), which will show the full call stack.

Now the NuGet addin will check if the exception thrown, whilst checking for updates, has an InnerException and if the error message
is the one thrown by the DispatchService and will show the inner exception's message instead in the Package Console.

