---
layout: post
title: "ASP.NET 5 and DNX support in Xamarin Studio"
date: 2016-01-01 12:00
comments: true
categories: DNX Xamarin MonoDevelop
---

Xamarin Studio and MonoDevelop now have support for [ASP.NET 5 and DNX](http://docs.asp.net/en/latest/dnx/overview.html) with an alpha release of the [DNX addin](https://github.com/mrward/monodevelop-dnx-addin).

{% img /images/blog/DNXSupportInXamarinStudio/DnxWebProjectInSolutionWindow.png 'DNX web project in Solution window' 'DNX web project in Solution window' %}

The addin uses code from [OmniSharp](https://github.com/OmniSharp/omnisharp-roslyn) in order to communicate with the DNX host.

## Features

   * Project templates for console, library and web applications
   * Debugger support with Mono 4.3
   * Code completion
   * NuGet integration
   * Dependencies shown in Solution window

## Supports 

   * MonoDevelop and Xamarin Studio 5.9 and above.
   * ASP.NET 5 RC 1 Update 1 and earlier versions.

## Installing ASP.NET 5

ASP.NET 5 needs to be installed separately before using the DNX addin. There are instructions on [get.asp.net](https://get.asp.net/) on how to do this for Mac, Linux and Windows.

## Installing the addin

The addin is currently available from [MonoDevelop's Add-in Repository](http://addins.monodevelop.com/) in the alpha channel. In Xamarin Studio open the Add-in Manager and select the Gallery tab. Click the repository drop down and if **Xamarin Studio Add-in Repository (Alpha Channel)** is not displayed then click **Manage Repositories...**. In the window that opens tick the check box next to **Xamarin Studio Add-in Repository (Alpha Channel)** and then click the Close button.

{% img /images/blog/DNXSupportInXamarinStudio/AddingAlphaChannelAddins.png 'Enabling alpha channel addins' 'Enabling alpha channel addins' %}

Back in the Add-in Manager dialog click the Refresh button to update the list of addins. Use the search text box in the top right hand corner of the dialog to search for the addin by typing in **DNX**.

{% img /images/blog/DNXSupportInXamarinStudio/AddinManagerDNXAddin.png 'DNX addin selected in Addin Manager dialog' 'DNX addin selected in Addin Manager dialog' %}

Select the DNX addin and then click the **Install...** button.

After installing the DNX addin you will need to restart Xamarin Studio before the project templates will be available in the New Project dialog.

## Creating an ASP.NET 5 project

There are three project templates available for ASP.NET 5 in the New Project dialog. 

{% img /images/blog/DNXSupportInXamarinStudio/NewProjectDialogDnxProjectTemplates.png 'New ASP.NET 5 project templates' 'New ASP.NET 5 project templates' %}

Each of the project templates will create a solution and an .xproj file in the same way as Visual Studio 2015 does. The projects created should be compatible with Visual Studio 2015.

{% img /images/blog/DNXSupportInXamarinStudio/DnxConsoleProjectInSolutionWindow.png 'DNX console project in Solution window' 'DNX console project in Solution window' %}

## Dependencies

When you create or open an ASP.NET 5 project the NuGet packages it uses will automatically be restored. 

{% img /images/blog/DNXSupportInXamarinStudio/SolutionWindowRestoringNuGetPackages.png 'Restoring NuGet packages in Solution window' 'Restoring NuGet packages in Solution window' %}

In the Solution window the dependencies show a warning icon if they are not resolved and the Dependencies folder shows a message that it is restoring the NuGet packages. Once restored you can expand the Dependencies folder and see the dependency hierarchy.

{% img /images/blog/DNXSupportInXamarinStudio/DnxConsoleProjectDependenciesExpanded.png 'Dependency hierarchy in Solution window' 'Dependency hierarchy in Solution window' %}

If the DNX runtime being used by the solution is not installed then an error icon will be displayed next to the Dependencies folder.

{% img /images/blog/DNXSupportInXamarinStudio/SolutionWindowDependenciesError.png 'Dependencies error icon in Solution window' 'Dependencies error icon in Solution window' %}

Hovering the mouse over the error icon will show more information about the error.

## Building

When you build the solution or project any errors and warnings are displayed in the Errors window and in the text editor. No assemblies are generated when you build.

{% img /images/blog/DNXSupportInXamarinStudio/TextEditorDnxBuildError.png 'DNX build error in text editor' 'DNX build error in text editor' %}

{% img /images/blog/DNXSupportInXamarinStudio/DnxBuildErrorsInErrorsWindow.png 'DNX build errors in Errors window' 'DNX build errors in Errors window' %}

## Running

The main toolbar shows the currently selected command and the framework that will be used when the application is run. These are taken from the project.json file.

{% img /images/blog/DNXSupportInXamarinStudio/DnxCommandsInMainToolbar.png 'DNX commands in main toolbar' 'DNX commands in main toolbar' %}

The screenshot above shows the main toolbar for a web project. There are three entries:

   * web
   * web DNX 4.5.1
   * web DNX Core 5.0

The first entry will run the web command and use the default runtime which will currently be DNX 4.5.1. The last two entries will run the web command but will explicitly run the application with the specified runtime.

## Debugging

Thanks to [David Karlaš](https://twitter.com/davidkarlas) and [Zoltan Varga](https://github.com/vargaz) there is support for debugging DNX applications if you have [Mono 4.3](http://download.mono-project.com/archive/nightly/macos-10-universal/) installed.

{% img /images/blog/DNXSupportInXamarinStudio/DebuggingDnxApplication.png 'Debugging a DNX application' 'Debugging a DNX application' %}

Debugging ASP.NET 5 projects running with the CoreCLR is not supported on Mac nor on Linux. Debugging ASP.NET 5 projects it not supported at all on Windows.

## Adding NuGet Packages

To add a NuGet package to a project you can right click the Dependencies folder in the Solution window and select Add NuGet Packages, or alternatively you can double click the Dependencies folder.

{% img /images/blog/DNXSupportInXamarinStudio/AddNuGetPackagesMenuItem.png 'Add NuGet Packages menu in Solution window' 'Add NuGet Packages menu in Solution window' %}

This will open up the Add NuGet Packages dialog. Installing a NuGet package using this dialog will add the NuGet package into the project.json file.

Note that using the Add Packages dialog from the Packages folder will not add the NuGet package into the project.json file and will instead download the package to the packages directory and update the packages.config file.

## Adding Dependencies

Dependencies can also be added directly into the project.json file. Currently there is no code completion available in the project.json file but changes to the file are monitored and the Dependencies folder will be updated when items are added or removed. New NuGet package dependencies that are added will automatically be restored if they are missing.

## Removing Dependencies

A dependency can be removed by selecting it in the Solution window, right clicking and selecting Remove, or by pressing the delete key.

{% img /images/blog/DNXSupportInXamarinStudio/RemoveDependencyMenuItem.png 'Remove dependency menu in Solution window' 'Remove dependency menu in Solution window' %}

## Changing the active framework

In Visual Studio the active framework can be changed by selecting it from the drop down list at the top of the text editor. Currently using the drop down list at the top of the text editor to change the active framework is not supported in Xamarin Studio. Instead you can right click in the text editor, select Active Framework and then select the framework.

{% img /images/blog/DNXSupportInXamarinStudio/TextEditorSelectActiveDnxFramework.png 'Active DNX framework context menu in text editor' 'Active DNX framework context menu in text editor' %}

The active framework affects the code completion provided in the text editor. In the screenshot below the active framework has been set to DNX Core and you can see that the DateTime type does not show ToShortDateTimeString in its completion list.

{% img /images/blog/DNXSupportInXamarinStudio/DnxCodeCompletionForActiveFramework.png 'Code completion for active DNX framework' 'Code completion for active DNX framework' %}

You can also see that the code in the #if DNX451 block is grayed out since it is not used with the current active framework.

## DNX Output

Output from the DNX host and from OmniSharp can be seen in the DNX Output window. This window can be opened by selecting **DNX Output** from the **View** menu.

{% img /images/blog/DNXSupportInXamarinStudio/DnxOutputWindow.png 'DNX Output window' 'DNX Output window' %}

By default only warnings and errors will be displayed in the DNX Output window. To see more or less information you can change the DNX output verbosity in preferences in the DNX - General section.

## Known Issues

**kqueue() FileSystemWatcher has reached the maximum number of files to watch**
 
 When opening a project with many source files, such as AutoFac, you may see the above FileSystemWatcher error message. To resolve this problem you can set the MONO_MANAGED_WATCHER to false:

    export MONO_MANAGED_WATCHER=false

If this is set on the command line then Xamarin Studio would need to be run from the command line for this environment variable to be used.

**project.json file formatting**

When adding or removing dependencies using the IDE and not editing the project.json file will cause the project.json file to be reformatted different to how Visual Studio formats the file.

## Source Code

The source code for the addin is available on GitHub.

  - [DNX addin source code](https://github.com/mrward/monodevelop-dnx-addin).
 
 There is also a [separate branch](https://github.com/mrward/monodevelop-dnx-addin/tree/roslyn) that supports MonoDevelop and Xamarin Studio 6.0.
