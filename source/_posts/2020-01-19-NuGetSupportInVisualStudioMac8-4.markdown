---
layout: post
title: "NuGet Support in Visual Studio for Mac 8.4"
date: 2020-01-19 12:00
comments: true
categories: NuGet Xamarin VSMac MonoDevelop
---

## Changes

   * Improved accessibility of the Manage NuGet Packages dialog and Add Package Source dialog
   * Fixed updating multiple NuGet packages failing due to restricted version range of a common dependency
 
More information on all the new features and changes in [Visual Studio for Mac 8.4](https://www.visualstudio.com/vs/visual-studio-mac/)
can be found in the [release notes](https://docs.microsoft.com/en-us/visualstudio/releasenotes/vs2019-mac-relnotes).

## Accessibility Improvements - Manage NuGet Packages dialog

**Voice Over now recognises tabs**

The Browse, Installed, Updates and Consolidates tabs are now recognised as tabs
by Voice Over. Previously they were being treated as text labels and Voice Over
would not announce that a tab could be selected via Control-Option-Space.

**Voice Over now announces status messages**

The status message that is displayed with the busy spinner image is now
announced by Voice Over. When the status message is removed another
message is announced by Voice Over to say the loading has completed
or, if a search filter was entered, search was completed. If no
packages are found when searching this message is also announced
by Voice Over.

**Improve Voice Over reading of Id label**

The Id label has been changed to ID. Using ID allows Voice Over to announce it as I.D. instead of a single
word. Note that some Voice Over voices, such as English UK, still
announce the text incorrectly. Without changing the label text to be "I.D."
there does not seem to be a way of fixing this for all voices. System
Preferences on the Mac can be used to change the pronounciation for
certain words if required.

**Voice Over now reads first column header in Consolidate tab**

The check box to select a project to consolidate was in its own column that
had no name. Using Voice Over it was not obvious what this check box was for. Now the check
box and the project name are in the same column and Voice Over reads
the column header name, the project name and the checkbox state which
makes it easier to understand what the list is displaying.

**Voice Over now reading associated labels for selected UI control**

Labels were not associated with their corresponding UI controls which
resulted in them not being read by Voice Over when corresponding UI control
was selected.

The package source combo box is now read by Voice Over as 'Package Sources'.
Previously there was no associated label information being read.

Voice Over now reads the label for Search text field. The search text box now shows 'Search' as
placeholder text.

**Voice Over now reading package list information**

Voice Over now reads the package information in the list and the check box
can be ticked or unticked by using Voice Over. Previously no information was
read by Voice Over and the check box could not be accessed.

**Keyboard can now be used to move across tabs** 

Previously it was not possible to tab across the Browse, Installed, Updates or Consolidate tabs.

This change will be in Visual Studio for Mac 8.4.2.

### Accessibility Improvements - Add Package Source Dialog

In Preferences - NuGet - Sources, the Add Package Source dialog
would lose focus when the Browse button was used. A way to reproduce
this is to tab to the Add button, press space, then tab to the
Browse button in the Add Package Source dialog, press space, then
press escape. The Add Package Source dialog then no longer has focus
and tabbing does not work.

This has been fixed by explicitly setting focus back to the parent dialog
when a child dialog is closed.

## Bug Fixes

**Fixed updating multiple NuGet packages failing due to restricted version range of a common dependency**

Updating two NuGet package references that have a strict dependency
on a single version of another NuGet package, not explicitly added
to the project, would fail in the Manage NuGet Packages dialog. This
was because the update was being done one NuGet package at a time.

With an Android project that uses Xamarin.Forms this could happen when updating
Xamarin.Essentials, an Xamarin.Android.Support.Core.Utils and Xamarin.Forms at the same time.
The Xamarin.Android.Support NuGet packages
depend on specific versions for their dependencies which can cause the update to fail
depending on whether a NuGet package is updated on its own or in a group. The resulting failure
would be similar to

```
Version conflict detected for Xamarin.Android.Support.Collections. Install/reference Xamarin.Android.Support.Collections 28.0.0.3 directly to project to resolve this issue. 
 MyProject.Android -> Xamarin.Android.Support.Core.Utils 28.0.0.3 -> Xamarin.Android.Support.Compat 28.0.0.3 -> Xamarin.Android.Support.Collections (= 28.0.0.3) 
 MyProject.Android -> Xamarin.Essentials 1.2.0 -> Xamarin.Android.Support.CustomTabs 28.0.0.1 -> Xamarin.Android.Support.Collections (= 28.0.0.1).
```

Updating NuGet packages in the Manage NuGet packages dialog is now done
as a batch instead of individually. This allows NuGet to
update both NuGet packages so they then use the new strict dependency.

Note that updating all packages in the project from the Solution
window does not have this problem since there the packages were already
updated together in a batch.





