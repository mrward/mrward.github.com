---
layout: post
title: "NuGet Support in Visual Studio for Mac 7.7"
date: 2018-12-19 14:40
comments: true
categories: NuGet Xamarin VSMac MonoDevelop
---

## Changes

   * NuGet 4.8 support
   * Support PackageReferences without a Version
   * Fixed NuGet sdk resolver not being found in Mono 5.16
   * Fixed null reference exception in package compatiblity check
   * Fixed Update menu enabled when project has no PackageReferences
   * Fixed updating a NuGet package changing a reference's ItemGroup
   * Fixed updating a NuGet package changing a fully qualified reference hint path to a relative path

More information on all the new features and changes in [Visual Studio for Mac 7.7](https://www.visualstudio.com/vs/visual-studio-mac/)
can be found in the [release notes](https://docs.microsoft.com/en-us/visualstudio/releasenotes/vs2017-mac-relnotes#whats-new-in-77).

## NuGet 4.8 support

[NuGet 4.8.0.5385](https://docs.microsoft.com/en-us/nuget/release-notes/nuget-4.8-rtm) is now
included with Visual Studio for Mac 7.7.2.

## Support PackageReferences without a Version

Visual Studio for Mac did not support projects that used PackageReferences without
specifying a version.
    
    <ItemGroup>
        <PackageReference Include="Newtonsoft.Json" />
    </ItemGroup>

The version may be defined elsewhere in another MSBuild file, such as the Directory.props file,
or by the .NET Core SDK, as with the 
Microsoft.AspNetCore.App PackageReference in ASP.NET Core projects. By default a 
PackageReference without a version will restore the lowest available version for
the NuGet package.

However in Visual Studio for Mac there were several problems with PackageReferences that
did not specify a version.

Opening a project with a PackageReference without a Version
would result in an ArgumentNullException being logged and the
Add Packages dialog could not be opened.
    
If the PackageReference, in a non-SDK project, had no Version
then it was not displayed in the Packages folder and a null reference
exception was logged. The Solution window would try to find
the package to check if it was installed which is not possible
with a missing version and NuGet's VersionFolderPathResolver
would throw a null reference exception.
   
If a PackageReference had no Version then a null reference exception
was logged when checking for updates. A null version is now handled.

Right clicking the package in the Packages folder would log a
null reference exception if a non-SDK style project was used and it
had a PackageReference without a version. This is now handled and the
menu label will show "Version None".

## Bug Fixes

**Fixed NuGet sdk resolver not being found in Mono 5.16**
    
More recent versions of MSBuild, such as MSBuild 16.0.40 which is included
with Mono 5.16.0.173, allow the sdk resolver to use a manifest.xml file to
define the assembly where the resolver can be found:
    
    <SdkResolver>
      <Path>..\..\Microsoft.Build.NuGetSdkResolver.dll</Path>
    </SdkResolver>

This manifest file not supported and resulted in the NuGet sdk resolver not
being loaded. Any projects that use an MSBuild sdk from a NuGet
package no longer worked and would result in an 'Invalid configuration
mapping' error shown in the Solution window. The sdk resolution in
Visual Studio for Mac has now 
been updated based on the latest MSBuild source code.

**Fixed null reference exception in compatiblity check**

Changing the target framework of a project that uses a packages.config
file will result in a package compatiblity check being run.
If the project had both a PackageReference and a packages.config file
the package compatiblity check would fail with a null
reference exception. Visual Studio for Mac was treating the project as
though it was using a packages.config file, when it should have been
treated as a PackageReference project. This resulted in a null
reference exception being thrown when checking for package compatiblity.

**Fixed Update menu enabled when project has no PackageReferences**
    
The Update menu was enabled if the project used PackageReferences but
had none in the project. Without any PackageReferences in the
project there is no packages to update. The check to determine if the Update
menu should be enabled has been changed to make sure the project has
PackageReferences in the project file, not just imported
PackageReferences. The Update
NuGet Packages menu, which is used to update packages for the solution,
has also been changed to have the same behaviour.

**Fixed updating a NuGet package changing a reference's ItemGroup**

On updating a NuGet package the Reference item will now be modified in 
place in the project file.

On updating a NuGet package in a project that used a packages.config
the old NuGet package is uninstalled and the new one is installed.
This removes the old references and adds new references. If the
references are in an ItemGroup with a condition then the new
reference may be added into a different ItemGroup if there are other
ItemGroups with references. To prevent this from happening the
changes to made to references are cached and not applied to the
project until all the NuGet actions have all been run. This allows
a NuGet package update which would remove a reference and then
add a new reference to be
converted into an update of the original Reference in the project,
changing just its HintPath, so its
location in the project file is not changed.

This was fixed in Visual Studio for Mac 7.7.3.

**Fixed updating a NuGet package changing a fully qualified reference hint path to a relative path**

Updating a NuGet package, where the project, which uses a packages.config file,
had been modified so the
original reference had a fully qualified hint path, would result in
a relative path used for the hint path when the reference was updated.

Now if the original hint path was full path then if the hint
path for the reference is changed it is saved using a full
path. This is different behaviour to how Visual Studio on Windows works, which
will always add a relative hint path for the reference.

This was fixed in Visual Studio for Mac 7.7.3.