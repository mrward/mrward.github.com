---
layout: post
title: "File Nesting in Xamarin Studio"
date: 2016-07-16 14:00
comments: true
categories: Xamarin MonoDevelop
---

In recent versions of Xamarin Studio it is not currently possible to nest files without directly editing the project file. In the past it was possible to drag and drop a file so it was then nested inside another file.

{% img /images/blog/FileNestingSupportInXamarinStudio/SolutionWindowWithNestedFiles.png 'Solution window with nested files in Xamarin Studio' 'Solution window with nested files in Xamarin Studio' %}

Visual Studio also does not support nesting files by dragging and dropping however 
[Mads Kristensen](http://madskristensen.net/) created a [File Nesting extension](https://visualstudiogallery.msdn.microsoft.com/3ebde8fb-26d8-4374-a0eb-1e4e2665070c) that adds support for manual and automatic nesting of files within Visual Studio. This extension has now been ported to Xamarin Studio and is available from the [MonoDevelop Add-in Repository](http://addins.monodevelop.com/).

Let us take a walkthrough of the features of the File Nesting addin for Xamarin Studio.

## Features

 - Manually nest files
 - Manually un-nest files
 - Automatic file nesting selected files based on rules
 - Automatic file nesting when files are added or renamed
 - Options to specify which file nesting rules are applied

Mads Kristensen has an [Introducing File Nesting extension for Visual Studio demo video](https://channel9.msdn.com/Blogs/MadsKristensen/Introducing-File-Nestor-for-Visual-Studio) that shows the extension being used with Visual Studio.

## Supports

  - Xamarin Studio 6.0 or MonoDevelop 6.0.

## Manual File Nesting

To manually nest a file select it in the Solution window then right click and select File Nesting - Nest Item...

{% img /images/blog/FileNestingSupportInXamarinStudio/NestItemContextMenu.png 'Manual file nesting - nest item context menu' 'Manual file nesting - nest item context menu' %}

This will open a file nesting dialog where the parent file can be selected.

{% img /images/blog/FileNestingSupportInXamarinStudio/FileNestingDialog.png 'File nesting dialog' 'File nesting dialog' %}

Select the parent file and click OK to nest the file under that parent file.

{% img /images/blog/FileNestingSupportInXamarinStudio/ManualFileNestingResultInSolutionWindow.png 'Manual file nesting result in solution window' 'Manual file nesting result in solution window' %}

You can also nest multiple files under a parent by selecting multiple files in the Solution window and selecting the Nest Item menu.

## Manual File Un-nesting

To un-nest a file select it in the Solution window then right click and select File Nesting - Un-nest-Item.

{% img /images/blog/FileNestingSupportInXamarinStudio/UnNestItemContextMenu.png 'Manual file nesting - un-nest item context menu' 'Manual file nesting - un-nest item context menu' %}

The file will then be un-nested from its parent.

{% img /images/blog/FileNestingSupportInXamarinStudio/ManualFileUnNestingResultInSolutionWindow.png 'Manual file un-nesting result in solution window' 'Manual file un-nesting result in solution window' %}

## Automatic Nesting Rules

The file nesting rules are available from the Preferences dialog.

{% img /images/blog/FileNestingSupportInXamarinStudio/FileNestingRulesInPreferencesDialog.png 'File nesting rules in preferences' 'File nesting rules in preferences' %}

Each option has a tooltip which will show more detailed information about what the rule does.

### Enable auto-nesting

The Enable auto-nesting option will enable or disable automatic file nesting when a file is added to a project.

### Enable extension rule

Files added with an extension nest under their parent. For example MyView.xaml.cs nests under MyView.xaml.

{% img /images/blog/FileNestingSupportInXamarinStudio/NestedFileUsingExtensionRule.png 'Nested file using extension rule' 'Nested file using extension rule' %}

### Enable interface implementation rule

This nests C# interface implementations under this respective interfaces by filename. For example, if there is an interface file IMyInterface.cs then a new file called CustomMyInterface.cs will be nested under the interface.

{% img /images/blog/FileNestingSupportInXamarinStudio/NestedCSharpInterfaceFilesInSolutionWindow.png 'Nested file using interface implementation rule' 'Nested file using interface implementation rule' %}

### Enable known file type rule

This nests known files types, for example, MyPage.ts will be nested under MyPage.html.

{% img /images/blog/FileNestingSupportInXamarinStudio/NestedFileUsingKnownFileTypeRule.png 'Nested file using known file type rule' 'Nested file using known file type rule' %}

### Enable path segment rule

This nests files with an added path segment under its parent. For example, MyFile.Designer.cs nests under MyFile.cs.

{% img /images/blog/FileNestingSupportInXamarinStudio/NestedFileUsingPathSegmentRule.png 'Nested file using path segment rule' 'Nested file using path segment rule' %}

## Automatic Nesting of Selected Files

To automatically nest files, based on the enabled file nesting rules, select the files, or folder or the project then right click and select File Nesting - Auto-nest selected items.

{% img /images/blog/FileNestingSupportInXamarinStudio/AutoNestSelectedItemsContextMenu.png 'Auto-nest selected items context menu' 'Auto-nest selected items context menu' %}

This will then apply file nesting to the selected files.

## Automatic File Nesting on Adding Files

To enable automatic file nesting when files are added right click the project and select File Nesting - Enable automatic nesting.

{% img /images/blog/FileNestingSupportInXamarinStudio/EnableAutomaticNestingContextMenu.png 'Enable automatic nesting context menu' 'Enable automatic nesting context menu' %}

A check box will be displayed next to this menu item if this feature is enabled. Now when a file is added the enabled nesting rules will be applied and the file will be automatically nested.

## Installation

The File Nesting addin is available from the [MonoDevelop Add-in Repository](http://addins.monodevelop.com/). To install it open the Add-in Manager, search for the File Nesting addin, then click the Install button.

SCREENSHOT of addin.

After installing the addin you should restart Xamarin Studio for the addin to work correctly.

## Source Code

  - [File Nesting addin for Xamarin Studio and MonoDevelop](https://github.com/mrward/FileNesting)
  - [File Nesting extension for Visual Studio](https://github.com/madskristensen/FileNesting)


