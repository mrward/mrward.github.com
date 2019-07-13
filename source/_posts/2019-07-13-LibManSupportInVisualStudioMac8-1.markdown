---
layout: post
title: "LibMan support in Visual Studio for Mac"
date: 2019-07-13 12:00
comments: true
categories: Xamarin MonoDevelop VSMac
---


The [Library Manager addin](https://github.com/mrward/LibraryManager) provides 
Microsoft Library Manager (LibMan) support for Visual Studio for Mac. LibMan provides a way to install
third-party client-side JavaScript libraries for ASP.NET Core and ASP.NET projects.

## Features

 - Add Client-Side Library dialog
 - Restore client-side libraries
 - Deleting client-side libraries
 - Automatic client-side library restore on saving libman.json file

Full text editor support is not currently available. The following text editor quick actions are not supported:

 - Uninstall a client-side library
 - Check for client-side library updates

## Supports

 - Visual Studio Mac 8.1 or later.
 - ASP.NET Core and ASP.NET projects

## Add Client-Side Library Dialog

To open the Add Client-Side Library dialog, right click the project, or a folder, and select Add - Client-Side Library.

{% img /images/blog/LibManSupportInVisualStudioMac8-1/AddClientSideLibraryMenuItem.png 'Add - Client-Side Library menu' 'Add - Client-Side Library menu' %}

{% img /images/blog/LibManSupportInVisualStudioMac8-1/AddClientSideLibraryDialog.png 'Add Client-Side Library dialog' 'Add Client-Side Library dialog' %}

The library provider can be selected from the Provider list.

Typing in the Library text field will search the library provider and show a list of matching libraries.

{% img /images/blog/LibManSupportInVisualStudioMac8-1/AddClientSideLibraryDialogCompletion.png 'Add Client-Side Library dialog completion list' 'Add Client-Side Library dialog completion list' %}

Pressing tab or return will insert the selected library from the completion list into the Library text field.

You can then choose to include all the client-side library files or a selection of those files.

{% img /images/blog/LibManSupportInVisualStudioMac8-1/AddClientSideLibraryDialogSelectFiles.png 'Add Client-Side Library dialog select files' 'Add Client-Side Library dialog select files' %}

The Target Location indicates where the client-side library files will be installed.

Clicking the Install button will create a libman.json file and install the client-side library into your project.

{% img /images/blog/LibManSupportInVisualStudioMac8-1/LibManJQueryInstalledInProject.png 'jQuery installed into project' 'jQuery installed into project' %}

More detailed information about the client-side library installation is available by clicking the status bar or by selecting
View - Pads - Library Manager Output.

{% img /images/blog/LibManSupportInVisualStudioMac8-1/LibraryManagerOutputWindow.png 'Library Manager Output window' 'Library Manager Output window' %}

## Adding a libman.json file

To add a libman.json file without using the Add Client-Side Library dialog, right click the ASP.NET project and select
Manage Client-Side Libraries.

{% img /images/blog/LibManSupportInVisualStudioMac8-1/ManageClientSideLibrariesMenu.png 'Manage Client-Side Libraries menu' 'Manage Client-Side Libraries menu' %}

This will create a libman.json file and open it in the text editor.

{% img /images/blog/LibManSupportInVisualStudioMac8-1/LibManFileInTextEditor.png 'libman.json file in text editor' 'libman.json file in text editor' %}

## Restoring Client-Side Libraries

To restore the client-side libraries you can right click the libman.json file in the Solution window and
select Restore Client-Side Libraries.

{% img /images/blog/LibManSupportInVisualStudioMac8-1/RestoreClientSideLibraries.png 'Restore Client-Side Libraries menu' 'Restore Client-Side Libraries menu' %}

Information about the restore operation is available from the Library Manager Output window.

Alternatively saving the libman.json file in the text editor will run a restore.

Restore errors are displayed in the Errors window and in the libman.json file if it is open in the text editor.

{% img /images/blog/LibManSupportInVisualStudioMac8-1/LibManRestoreError.png 'LibMan restore error' 'LibMan restore error' %}

## Deleting Client-Side Libraries

To remove the client-side libraries you can right click the libman.json file in the Solution window and
select Clean Client-Side Libraries.

{% img /images/blog/LibManSupportInVisualStudioMac8-1/CleanClientSideLibrariesMenu.png 'Clean Client-Side Libraries menu' 'Clean Client-Side Libraries menu' %}

This will delete the client-side libraries from the project.

## Library Manager Addin Installation

The Library Manager addin is available from the Visual Studio for Mac extensions repository. To install the addin:

 - From the main menu, open the Extensions Manager dialog.
 - Select the Gallery tab.
 - Expand IDE extensions.
 - Select the Library Manager addin
 - Click the Refresh button if the addin is not visible.
 - Click Install… to install the addin.
 - Restart Visual Studio for Mac.

## Links

 - [Library Manager for Visual Studio for Mac](https://github.com/mrward/LibraryManager)
 - [Use LibMan with ASP.NET Core in Visual Studio](https://docs.microsoft.com/en-us/aspnet/core/client-side/libman/libman-vs)
