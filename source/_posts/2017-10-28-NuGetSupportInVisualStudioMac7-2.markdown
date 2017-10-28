---
layout: post
title: "NuGet Support in Visual Studio for Mac 7.2"
date: 2017-10-28 12:00
comments: true
categories: NuGet Xamarin VSMac MonoDevelop
---

## Changes

   * NuGet 4.3.1 support
   * NuGet fallback folders support
   * AssetTargetFallback support
   * NuGet operations can be cancelled on closing the solution
   * Fixed transitive types from references not being available
   * Fixed credential dialog being shown multiple times on opening a solution

More information on all the new features and changes in [Visual Studio for Mac 7.2](https://www.visualstudio.com/vs/visual-studio-mac/) can be found in the [release notes](https://www.visualstudio.com/en-us/news/releasenotes/vs2017-mac-relnotes#release-date-october-9-2017---visual-studio-2017-for-mac-720636).

## NuGet 4.3.1 support

NuGet 4.3.1.4445 is now included with Visual Studio for Mac 7.2.

NuGet 4.3.1 includes [a fix for imports in the project.json file being ignored](https://github.com/NuGet/Home/issues/5806) which could cause a NuGet package
to incorrectly be considered incompatible when restoring NuGet packages.

## NuGet package fallback folders support

The .NET Core SDK 2.0 defines a NuGet package fallback folder `/usr/local/share/dotnet/sdk/NuGetFallbackFolder` that can be used when looking
for NuGet packages whilst restoring. This fallback folder is now supported by
Visual Studio for Mac 7.2 so that on restoring a .NET Core 2.0 project the NuGet
packages from the fallback folder will be found and do not need to be downloaded from nuget.org into the 
local machine NuGet package cache `~/.nuget/packages`. This should speed up NuGet package restore for .NET Core 2.0 and .NET Standard 2.0 projects the first time it occurs.

As well as the NuGet fallback folders Visual Studio for Mac will now add the following items to the generated project.assets.json
if they are available:

 - configFilePaths
 - sources
 - warningProperties

## AssetTargetFallback support

The .NET Core 2.0 SDK uses an AssetTargetFallback MSBuild property
defined in an imported SDK MSBuild files. This is used instead
of the PackageTargetFallback property when determining if a NuGet
package is compatible. Currently the AssetTargetFallback property
is set to net461 by the .NET Core 2.0 SDK which allows .NET Core projects to use NuGet
packages that include assemblies that target the full .NET Framework.
The supported fallback frameworks are now added to the generated
project.assets.json file by Visual Studio for Mac when a .NET Core 2.0 project is restored.

## NuGet operations can be cancelled on the closing the solution

Previously when closing the solution, or closing Visual Studio for Mac, when a NuGet package
operation was still running would result in a dialog being displayed saying that the
solution cannot be closed until the NuGet operation was completed.
NuGet v3 and above now allow the NuGet
operations to be cancelled so now the dialog allows the current
operation to be cancelled. If the
operation is taking a while to cancel a busy spinner image will be displayed in
the dialog.

{% img /images/blog/NuGetSupportInVisualStudioMac7-2/CancelNuGetOperationOnClosingSolutionDialog.png 'Cancel NuGet operation dialog on closing solution' 'Cancel NuGet operation dialog on closing solution' %}

If a restore is being run when the solution
will be closed the restore will be cancelled automatically without showing the dialog.

## NuGet package restore now fails if package and asset target fallbacks are defined by a project

If both AssetTargetFallback and PackageTargetFallback are defined by a project then the
NuGet restore will fail with an error indicating that they cannot
be used together. This mirrors the behaviour of the .NET Core command
line restore.

## Support imported package references in non .NET Core projects

If a non .NET Core project had no PackageReference items but imported a file that
did have PackageReference items the NuGet packages were not restored
on opening the solution. This was because only the PackageReference
items defined directly in the project were checked to determine if the
project used NuGet packages. Now the evaluated MSBuild items are checked
so any imported PackageReference items are detected and a restore
will be run on opening the solution.

Note that imported PackageReference items are not displayed in the
Packages folder.

## Package Console is no longer opened when a NuGet operation is cancelled

When a NuGet operation is cancelled from the main status bar the
Package Console is now not opened when the NuGet operation fails due
to the cancellation. If the NuGet operation is not cancelled then the
Package Console window will still be opened as before if the operation fails.
This is also a workaround for Visual Studio for Mac closing the currently displayed
dialog when the Package Console window opens after the NuGet operation
is cancelled when closing the solution.

## Whitespace is now trimmed when creating a new package source

When creating a new package source copying and pasting NuGet package source url can sometimes copy extra
whitespace which can then result in NuGet package restore errors such as:

    Failed to verify the root directory of local source
    ' https://api.nuget.org/v3/index.json'.

The package source name and url will now have whitespace trimmed to avoid
this copy and paste problem. This also
matches Visual Studio on Windows behaviour.

## Mark implicit PackageReferences as auto referenced

PackageReference items that have IsImplicitlyDefined set to true
in their metadata now have autoRefererenced set to true in the project.assets.json file.

## Bug Fixes

**Fixed transitive assembly references not available until restart**

Given a solution that contains three .NET Standard projects: LibC
references LibB which references LibA. If the Newtonsoft.Json NuGet
package was installed into LibA the types from this NuGet package
were not available in LibB or LibC until the solution was closed and
re-opened again. Closing and re-opening the solution refreshes the
reference information used by the type system. Now when a NuGet
package is installed into a .NET Core project the projects that
reference this project have their reference information refreshed. Types from
the installed NuGet packages are then available in projects that
reference this updated project either directly or indirectly.

**Fixed transitive project references after editing a project file**

Given a solution that contains three .NET Standard projects: LibC references
LibB which references LibA. If a NuGet package is added to LibA by
editing its project file in the text editor the types from this NuGet
package were not available to LibB or LibC without restarting Visual Studio 
for Mac or until the packages were restored for the solution. Now when the project file
is saved the projects that directly or indirectly reference the project
will be restored.

**Fixed credential dialog shown multiple times on opening a solution**

With check for updates enabled, multiple projects in the solution,
and a package source that was missing or had invalid credentials,
on opening the solution the credential dialog would be displayed multiple
times even if the correct username and password was entered or the
dialog was cancelled. The dialog was being displayed for each
project and the credential information was not being re-used.

Now the NuGet source repositories are re-used when checking
for updates and also when restoring the projects when the solution
is first opened. Any valid credentials entered will be re-used
when checking the remaining projects. If the credential dialog is
cancelled then the dialog is no longer displayed again whilst
checking for updates for the other projects.

**Fixed credential dialog displayed when credentials are available**

With valid credentials stored in the Mac key chain the credential dialog would
still be displayed when it should not have been. The problem was that the
NuGet credential service puts itself in a retry mode if any of the
credential providers are used when trying to authenticate against a
package source. Once in this retry mode the Visual Studio for Mac credential
provider would always show a dialog asking for the credentials instead
of re-using the existing credentials. To avoid this the credential
service is reset before any user actions, such as opening the Add Packages
dialog, running a restore or an update.

**Fixed crash when displaying Chinese characters in the Add Packages dialog**

With the NuGet v3 package source https://api.nuget.org/v3/index.json
selected, searching for Microsoft.AspNet.WebApi.Client would result in
the Visual Studio for Mac terminating when an attempt was made to display the results
in the Add Packages dialog.
The crash was in the Pango library when it attempted to determine the size of
the package title displayed in the search results. If the package
title contained Chinese characters then Pango would throw an exception:

    Illegal byte sequence encounted in the input.
    at (wrapper managed-to-native) Pango.Layout:pango_layout_get_pixel_size (intptr,int&,int&)

Then when Visual Studio for Mac tried to use Pango again to determine the
size of the text this would result in Visual Studio for Mac terminating.

```
Pango-WARNING (recursed) **: shaping failure, expect ugly output.
shape-engine='BasicEngineCoreText', font='.SF NS Text',
text='∫ê'Stacktrace:

  at <unknown> <0xffffffff>
  at (wrapper managed-to-native) Pango.Layout.pango_layout_get_pixel_size (intptr,int&,int&) [0x0000b] in <c7aa2d93df4045df8dc71d5439f99d72>:0
```

If the NuGet v2 package source was used then this crash did not occur
since the package title was not returned in the search results and the package id would be
displayed instead which does not contain Chinese characters.

As a workaround the Add Packages dialog now displays the package id
instead of the package title.

**Ignore project references with ReferenceOutputAssembly set to false when restoring**

Project references that have ReferenceOutputAssembly are now not
added to the project.assets.json file. This was causing
the NuGet package restore to fail in some cases. For example, if a .NET Standard 
project has a project reference to a .NET Core App project, but has the
ReferenceOutputAssembly set to false, then running dotnet restore from the
command line would work, but the restore would fail in Visual Studio for Mac.






