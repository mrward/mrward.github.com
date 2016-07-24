---
layout: post
title: "File Nesting in Xamarin Studio"
date: 2016-07-24 21:00
comments: true
categories: Xamarin MonoDevelop
---

With recent versions of Xamarin Studio it is not currently possible to nest files without directly editing the project file. In the past it was possible to drag and drop a file so it was then nested inside another file.

{% img /images/blog/FileNestingInXamarinStudio/SolutionWindowWithNestedFiles.png 'Solution window with nested files in Xamarin Studio' 'Solution window with nested files in Xamarin Studio' %}

Visual Studio also does not support nesting files by using drag and drop however 
[Mads Kristensen](http://madskristensen.net/) created a [File Nesting extension](https://visualstudiogallery.msdn.microsoft.com/3ebde8fb-26d8-4374-a0eb-1e4e2665070c) that adds support for manual and automatic nesting of files within Visual Studio. There is a [demo video of the File Nesting extension](https://channel9.msdn.com/Blogs/MadsKristensen/Introducing-File-Nestor-for-Visual-Studio) that shows the extension being used with Visual Studio.
 This extension has now been ported to Xamarin Studio and is available from the [MonoDevelop Add-in Repository](http://addins.monodevelop.com/).

Let us take a walkthrough of the features of the File Nesting addin for Xamarin Studio.

## Features

 - Manual file nesting
 - Manual file un-nesting
 - Automatic file nesting of selected files based on rules
 - Automatic file nesting when files are added to a project
 - Options to specify which file nesting rules are applied

## Supports

 - Xamarin Studio 6.0 or MonoDevelop 6.0.

## Manual File Nesting

To manually nest a file select it in the Solution window then right click and select File Nesting - Nest Item...

{% img /images/blog/FileNestingInXamarinStudio/NestItemContextMenu.png 'Manual file nesting - nest item context menu' 'Manual file nesting - nest item context menu' %}

This will open a file nesting dialog where the parent file can be selected.

{% img /images/blog/FileNestingInXamarinStudio/FileNestingDialog.png 'File nesting dialog' 'File nesting dialog' %}

Select the parent file and click OK to nest the file under that parent file.

{% img /images/blog/FileNestingInXamarinStudio/ManualFileNestingResultInSolutionWindow.png 'Manual file nesting result in solution window' 'Manual file nesting result in solution window' %}

You can also nest multiple files under a parent by selecting multiple files in the Solution window and selecting the Nest Item menu.

## Manual File Un-nesting

To un-nest a file select it in the Solution window then right click and select File Nesting - Un-nest Item.

{% img /images/blog/FileNestingInXamarinStudio/UnNestItemContextMenu.png 'Manual file nesting - un-nest item context menu' 'Manual file nesting - un-nest item context menu' %}

The file will then be un-nested from its parent.

{% img /images/blog/FileNestingInXamarinStudio/ManualFileUnNestingResultInSolutionWindow.png 'Manual file un-nesting result in solution window' 'Manual file un-nesting result in solution window' %}

## Automatic Nesting Rules

The file nesting rules are available from the Preferences dialog.

{% img /images/blog/FileNestingInXamarinStudio/FileNestingRulesInPreferencesDialog.png 'File nesting rules in preferences' 'File nesting rules in preferences' %}

Each rule has a tooltip which will show more detailed information about what the rule does.

### Enable auto-nesting

The Enable auto-nesting option will enable or disable automatic file nesting when a file is added to a project.

### Enable extension rule

This rule will nest files added with an extra extension under their corresponding parent file. For example MyView.xaml.cs nests under MyView.xaml.

{% img /images/blog/FileNestingInXamarinStudio/NestedFileUsingExtensionRule.png 'Nested file using extension rule' 'Nested file using extension rule' %}

### Enable interface implementation rule

This nests C# interface implementations under the corresponding interface based on the filename. For example, if there is an interface file IMyInterface.cs then a new file called CustomMyInterface.cs will be nested under the IMyInterface file.

{% img /images/blog/FileNestingInXamarinStudio/NestedCSharpInterfaceFilesInSolutionWindow.png 'Nested file using interface implementation rule' 'Nested file using interface implementation rule' %}

### Enable known file type rule

This nests certain known files types. For example, MyPage.ts will be nested under MyPage.html.

{% img /images/blog/FileNestingInXamarinStudio/NestedFileUsingKnownFileTypeRule.png 'Nested file using known file type rule' 'Nested file using known file type rule' %}

### Enable path segment rule

This nests files with an added path segment under its parent. For example, MyFile.Designer.cs nests under MyFile.cs.

{% img /images/blog/FileNestingInXamarinStudio/NestedFileUsingPathSegmentRule.png 'Nested file using path segment rule' 'Nested file using path segment rule' %}

## Automatic Nesting of Selected Files

To automatically nest files, based on the enabled file nesting rules, select the files, or folder, or project, then right click and select File Nesting - Auto-nest selected items.

{% img /images/blog/FileNestingInXamarinStudio/AutoNestSelectedItemsContextMenu.png 'Auto-nest selected items context menu' 'Auto-nest selected items context menu' %}

This will then apply the enabled file nesting rules to the selected files.

## Automatic File Nesting on Adding Files

To enable automatic file nesting when files are added right click the project and select File Nesting - Enable automatic nesting.

{% img /images/blog/FileNestingInXamarinStudio/EnableAutomaticNestingContextMenu.png 'Enable automatic nesting context menu' 'Enable automatic nesting context menu' %}

A check box will be displayed next to this menu item if this feature is enabled. Now when a file is added the enabled nesting rules will be applied and the file will be automatically nested.

## Installation

The File Nesting addin is available from the [MonoDevelop Add-in Repository](http://addins.monodevelop.com/) on the beta channel. To install the addin open the Add-in Manager, search for the File Nesting addin, then click the Install button.

{% img /images/blog/FileNestingInXamarinStudio/FileNestingAddinSelectedInAddinManagerWindow.png 'File Nesting addin in addin manager window' 'File Nesting addin in addin manager window' %}

After installing the addin Xamarin Studio will need to be restarted for the addin to work correctly.

## Source Code

 - [File Nesting addin for Xamarin Studio and MonoDevelop](https://github.com/mrward/FileNesting)
 - [File Nesting extension for Visual Studio](https://github.com/madskristensen/FileNesting)


