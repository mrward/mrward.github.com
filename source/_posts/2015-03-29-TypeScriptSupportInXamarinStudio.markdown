---
layout: post
title: "TypeScript Support in Xamarin Studio"
date: 2015-03-29 12:00
comments: true
categories: TypeScript Xamarin MonoDevelop
---

Xamarin Studio and MonoDevelop now have support for [TypeScript](http://www.typescriptlang.org/) on Linux, Mac and Windows with an alpha release of an [TypeScript Addin](https://github.com/mrward/typescript-addin).

The TypeScript addin uses [V8.NET](http://v8dotnet.codeplex.com) which is a library that allows a .NET application to host [Google's V8 JavaScript engine](https://code.google.com/p/v8/) and have JavaScript interact with .NET objects in the host application.

The ability to support Windows, Mac and Linux would not have been possible without the work done by [James Wilkins](http://jameswilkins.net) and [Christian Bernasko](https://github.com/chrisber).

[James Wilkins](http://jameswilkins.net) created the [V8.NET](http://v8dotnet.codeplex.com) library with the aim to have it work cross platform. When V8.NET was first released it supported only Windows. [Christian Bernasko](https://github.com/chrisber) then took V8.NET and got it working with Mono on Linux and the Mac. The TypeScript addin is using V8.NET binaries built by Christian from his port of [V8.NET](https://github.com/chrisber/v8dotnet/tree/development-mono).

Please note that this is an alpha release and because V8.NET uses a native library it can cause Xamarin Studio to terminate if there is a bug.

## Features

 * TypeScript compilation on save or build.
 * Code completion.
 * Find references.
 * Rename refactoring.
 * Go to declaration.
 * Errors highlighted as you type.
 * Code folding.
 
The addin supports:

 * Xamarin Studio MonoDevelop 5 and above.
 * TypeScript 1.4

## Installing the addin

The addin is currently available from [MonoDevelop's Add-in Repository](http://addins.monodevelop.com/) in its alpha channel. By default this alpha repository is not enabled so you will have to enable it before you can find the addin.

In Xamarin Studio open the Add-in Manager and select the Gallery tab. Click the repository drop down and if *Xamarin Studio Add-in Repository (Alpha Channel)* is not displayed then click *Manage Repositories...* in the drop down list. In the window that opens tick the check box next to *Xamarin Studio Add-in Repository (Alpha Channel)* and then click the Close button.

{% img /images/blog/TypeScriptSupportInXamarinStudio/AddingAlphaChannelAddins.png 'Enabling alpha channel addins' 'Enabling alpha channel addins' %}

Back in the Add-in Manager dialog click the Refresh button to update the list of addins. Use the search text box in the top right hand corner of the dialog to search for the addin by typing in **TypeScript**.

SCREENSHOT.

Select the TypeScript addin and then click the **Install...** button.

Note that if you are using Linux 32 bit then you should install the **TypeScript Linux 32 bit** addin. The other **TypeScript** addin listed supports Linux 64 bit. Hopefully in the future it will be possible to support both 32 bit and 64 bit using the same addin.

## Getting Started

Now that the TypeScript addin is installed let us create a TypeScript file.

TypeScript files are supported in any project type. To add a TypeScript file open the New File dialog, select the **Web** category and select Empty TypeScript file.

{% img /images/blog/TypeScriptSupportInXamarinStudio/NewFileDialogNewTypeScriptFile.png 'New File Dialog - New TypeScript File' 'New File Dialog - New TypeScript File' %}

Give the file a name and click the New button.

Note that currently the TypeScript file needs to be included in a project. Standalone TypeScript project files are not supported.

## Code Completion

When editing the TypeScript you will have code completion when you press the dot character.

{% img /images/blog/TypeScriptSupportInXamarinStudio/TypeScriptCodeCompletion.png 'TypeScript dot code completion' 'TypeScript dot code completion' %}

Code completion also works when you type the opening bracket of a function.

{% img /images/blog/TypeScriptSupportInXamarinStudio/TypeScriptMethodCompletion.png 'TypeScript method completion' 'TypeScript method completion' %}

## Go to Declaration

The text editor's right click menu has three TypeScript menus: Go to Declaration, Find References and Rename. 

{% img /images/blog/TypeScriptSupportInXamarinStudio/TypeScriptTextEditorContextMenu.png 'Text editor context menu with TypeScript menu options' 'Text editor context menu with TypeScript menu options' %}

The Go To Declaration menu option will open the corresponding definition in the text editor.

## Find References

Find References will show the reference locations in the Search Results window. 

{% img /images/blog/TypeScriptSupportInXamarinStudio/TypeScriptReferencesInSearchResults.png 'TypeScript references shown in Search Results window' 'TypeScript references shown in Search Results window' %}

## Rename

Selecting the Rename menu option in the text editor will open the Rename dialog box where you can type in the new name and click OK to have it updated everywhere it is referenced.

{% img /images/blog/TypeScriptSupportInXamarinStudio/TypeScriptRenameDialog.png 'TypeScript rename dialog' 'TypeScript rename dialog' %}

Note that currently on Linux the Rename dialog will only be displayed if the keyboard shortcut F2 is pressed. The context menu will not show the Rename dialog.

## Error Highlighting

Errors in your TypeScript code will be highlighted as you are typing in the text editor.

{% img /images/blog/TypeScriptSupportInXamarinStudio/TypeScriptErrorsHighlightedInTextEditor.png 'TypeScript errors highlighted in text editor' 'TypeScript errors highlighted in text editor' %}

## Code Folding

Code folding is supported for TypeScript classes, modules and interfaces. 

{% img /images/blog/TypeScriptSupportInXamarinStudio/TypeScriptCodeFolding.png 'TypeScript code folding' 'TypeScript code folding' %}

Code folding by default is disabled. To enable code folding open the Preferences dialog, in the Text Editor section select the General category and tick the **Enable code folding** check box.

{% img /images/blog/TypeScriptSupportInXamarinStudio/PreferencesEnableCodeFolding.png 'Preferences - Enabling code folding' 'Preferences - Enabling code folding' %}

## Compiling to JavaScript

By default the TypeScript files will be compiled to JavaScript when the project is compiled and saved next to the TypeScript files.

The compiler options are available in the project options in the Build category. 

{% img /images/blog/TypeScriptSupportInXamarinStudio/TypeScriptCompilerOptions.png 'TypeScript compiler options for the project' 'TypeScript compiler options for the project'%}

Here you can change when the compiler is run and what options are passed to the compiler when generating JavaScript code.

If an **Output file** is specified then all the TypeScript files will be compiled into a single JavaScript file. If an **Output directory** is specified then the JavaScript files will be generated in that directory instead of next to the TypeScript files.

## Source Code

 - [TypeScript addin source code](ttps://github.com/mrward/typescript-addin/tree/monodevelop-v8-dotnet).

  - [V8.NET source code](https://github.com/chrisber/v8dotnet/tree/development-mono).