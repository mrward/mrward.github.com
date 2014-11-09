---
layout: post
title: "Installing Portable Class Libraries for Xamarin Studio"
date: 2014-11-09 12:00
comments: true
categories: NuGet Xamarin PCL
---

In order to use Portable Class Libraries (PCLs) with Xamarin Studio you need to have the following installed:

 * Portable Class Library Reference Assemblies.
 * Portable Class Library MSBuild targets.
 * Xamarin's Portable Class Library Profiles.

If you do not all of the above installed then you may run into the following problems when using Xamarin Studio.

 1. Unable to create a Portable Library project since the project template is not available.
 2. Unable to install a NuGet package, such as Json.NET, that contains PCL assemblies into an Android or iOS project.

On the Mac the Portable Class Libraries for Mono 3.10.0 are installed into the directory:

    /Library/Frameworks/Mono.framework/Versions/3.10.0/lib/mono/xbuild-frameworks/.NETPortable

On Windows the Portable Class Libraries are installed into the directory:

    C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\.NETPortable
 
So let us take a look at how to install everything required to get PCLs fully supported within Xamarin Studio.

## Mac - Installing Portable Class Libraries

The Portable Class Libraries are included with the [Mono Development Kit (MDK)](http://www.mono-project.com/download/) but not with the Mono Runtime Environment (MRE).

If you are installing Mono yourself instead of using Xamarin's Universal Installer then you will want to install the MDK instead of the MRE. The MDK includes the MRE as well as extra tools, libraries and the .NET Portable Class Library profiles.

If Mono is updated from within Xamarin Studio using the "Check for updates" then the MDK should be installed.

## Windows - Installing Portable Class Libraries

Xamarin Studio on Windows uses Microsoft's .NET Framework and not Mono so the Portable Class Libraries need to be installed separately. To install the Portable Class Libraries on Windows you have two options:

  1. Install Visual Studio 2013 (full or [Express version](http://www.microsoft.com/en-us/download/details.aspx?id=43733)). Update 2 or above is required.
  2. Install the [Portable Library Tools](https://visualstudiogallery.msdn.microsoft.com/b0e0b5e9-e138-410b-ad10-00cb3caf4981/) and the [Portable Library Reference Assemblies 4.6](http://www.microsoft.com/en-us/download/details.aspx?id=40727).

If you do not want to install Visual Studio 2013 then you should look at option 2.

Let us take a look in more detail at the option 2 since this has a few manual steps.

First download the [Portable Library Tools](https://visualstudiogallery.msdn.microsoft.com/b0e0b5e9-e138-410b-ad10-00cb3caf4981/).

To install the Portable Library Tools open a command prompt and run:

    PortableLibraryTools /buildmachine

Now download the [Portable Library Reference Assemblies 4.6](http://www.microsoft.com/en-us/download/details.aspx?id=40727) and run the installer. This will install a PortableReferenceAssemblies.zip file into the directory:

    C:\Program Files (x86)\Microsoft .NET Portable Library Reference Assemblies 4.6

This PortableReferenceAssemblies.zip file contains three directories (4.0, 4.5 and 4.6) which need to be extracted and copied into the directory:

     C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\.NETPortable

If you installed Xamarin Studio before you installed the Portable Class Libraries you will now need to reinstall the Xamarin PCL profiles. The simplest way to do this is to find Xamarin in Add/Remove Programs, right click it and select Repair.