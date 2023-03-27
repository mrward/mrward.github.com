---
layout: page
title: "MonoDevelop .NET Core Extensions Addin Releases"
date: 2023-03-27 15:00
comments: true
sharing: true
footer: true
---

## Version 0.7

2023-03-27

 * Ensure solution extension is run before main .NET Core extension so it does not affect the built-in global.json support

## Version 0.6

2023-02-05

 * Support Visual Studio for Mac 17.5
 * Remove code to refresh ASP.NET Core project templates since this is no longer needed
 * Remove support for custom .NET CLI location in global.json file

## Version 0.5

2022-08-29

 * Allow active .NET runtime to be set for a solution from the Project - Active .NET menu
 * Allow global default .NET runtime to be set from the Project - Default .NET menu
 * Custom .NET runtimes can be specified in Preferences - Build and Debug - .NET Runtimes
 * Supports Visual Studio for Mac 17.4
 * Support custom .NET CLI location in the global.json file

## Version 0.4

2022-07-03

 * Support Visual Studio for Mac 17.3 preview 3

## Version 0.3

2022-06-12

 * Support Visual Studio for Mac 17.3 preview 2

## Version 0.2

2022-04-09

 * Support Visual Studio for Mac 17.0 preview 9
 * Support Arm64 SDKs on Apple Silicon machines
 * Fix SDK and runtime pkgs being downloaded when they are cached
 * Fix runtimes showing as pending uninstall after installing an SDK

## Version 0.1

2021-07-04

 * First Release
 