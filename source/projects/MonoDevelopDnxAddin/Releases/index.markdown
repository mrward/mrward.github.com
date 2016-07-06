---
layout: page
title: "MonoDevelop .NET Core Addin Releases"
date: 2016-07-06 21:50
comments: true
sharing: true
footer: true
---

## Version 0.5

2016-07-06

 * Add support for running unit tests with .NET Core test runners from the Unit Tests window with results shown in the Test Results window.
 * Update project templates for .NET Core 1.0 (preview 2)
 * Fix missing icons in New Project dialog in Xamarin Studio 6.1.
 * Parse build errors from dotnet build output.
 * Support projects created by VS 2015 Update 2.
 * Fix icon not displayed for clear button in .NET Core Output window.
 * Show design time host process output in .NET Core output window.
 * Fix package restore happening continually.
 * Fix errors not being cleared from Errors window.
 * Update status bar when restoring dependencies.
 * Fix dependencies not showing in Solution window on Linux.
 * Show error message if .NET Core cannot be found.

## Version 0.4

2016-06-12

 * Fix being unable to open a project created on Windows.
 * Do not show directories starting with dot in Solution window. 
 * Fix suppressed warnings still showing in text editor. 
 * New solution pad icons created by [Václav Vančura](https://github.com/vancura).
 
## Version 0.3

2016-06-05

 * Support .NET Core 1.0 RC2 SDK.
 * Supporting debugging .NET Core projects on Mac.
 * Fix UI hang if design time host does not return project information.

## Version 0.2

2016-03-28

 * Do not start restore if project.lock.json file changes.
 * Allow dependencies to be manually restored.
 * Fix incorrect project name being created when name contains dot character.
 * Watch files in directories defined in global.json file.
 * Do not show backup files in Solution window.
 * Show empty directories in Solution window.
 * Allow automatic DNX restore to be enabled or disabled.

## Version 0.1

2016-01-01

 * First Release
 