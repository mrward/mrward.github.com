---
layout: page
title: "NuGet File Locations"
date: 2016-05-14 12:24
comments: true
sharing: true
footer: true
---

NuGet directory and file locations across Linux, Mac and Windows operating systems.

## NuGet Cache

Stores downloaded NuGet packages (.nupkg).
 
**Mac**

 * ~/.local/share/NuGet/Cache
 * ~/.nuget/packages

**Windows**

 * %LocalAppData%\NuGet\Cache
 * %UserProfile%\\\\.nuget\packages

**Linux**

 * ~/.local/share/NuGet/Cache
 * ~/.nuget/packages 

## NuGet Configuration

The [NuGet.Config](http://docs.nuget.org/docs/reference/nuget-config-file) file stores user defined NuGet package sources, credentials and proxy settings. The default location for this file is:

**Mac** ~/.config/NuGet/NuGet.Config

**Windows** %AppData%\NuGet\NuGet.Config

**Linux** ~/.config/NuGet/NuGet.Config

## Machine Wide NuGet Configuration

Machine wide configurations are used to define NuGet package sources specific to a machine or a particular IDE, such as Visual Studio.

**Mac**

**Windows** %ProgramData%\NuGet\Config

**Linux**

Inside this directory there may be a NuGet.Config file or subdirectories for IDE specific NuGet.Config files. The files are checked for in the following order:

 1. %ProgramData%\NuGet\Config\IDE\Version\SKU\\*.config
 2. %ProgramData%\NuGet\Config\IDE\Version\\*.config
 3. %ProgramData%\NuGet\Config\IDE\\*.config
 4. %ProgramData%\NuGet\Config\\*.config
 