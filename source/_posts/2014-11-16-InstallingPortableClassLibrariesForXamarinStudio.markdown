---
layout: post
title: "Installing Portable Class Libraries for Xamarin Studio"
date: 2014-11-16 10:00
comments: true
categories: NuGet Xamarin PCL
---

In order to use Portable Class Libraries (PCLs) with Xamarin Studio you need to have the following installed:

 * Portable Class Library Reference Assemblies.
 * Portable Class Library MSBuild targets.
 * Xamarin's Portable Class Library Profiles.

If you do not all of the above installed then you may run into the following problems when using Xamarin Studio.

 1. Unable to create a Portable Library project since the project template is not available.
 2. Unable to install a NuGet package, such as Json.NET, that contains PCL assemblies into an Android or iOS project. Example error message below:
 
    Could not install package 'Newtonsoft.Json 6.0.6'. You are trying to install this package into a project that targets 'MonoAndroid,Version=v4.4', but the package does not contain any assembly references or content files that are compatible with that framework. For more information, contact the package author.

On the Mac the Portable Class Libraries for Mono 3.10.0 are installed into the directory:

    /Library/Frameworks/Mono.framework/Versions/3.10.0/lib/mono/xbuild-frameworks/.NETPortable

On Windows the Portable Class Libraries are installed into the directory:

    C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\.NETPortable
 
So let us take a look at how to install everything required to get PCLs fully supported within Xamarin Studio.

## Mac - Installing Portable Class Libraries

The Portable Class Libraries are included with the [Mono Development Kit (MDK)](http://www.mono-project.com/download/) but not with the Mono Runtime Environment (MRE). If you are installing Mono yourself instead of using Xamarin's Universal Installer then you will want to install the MDK instead of the MRE. The MDK includes the MRE as well as extra tools, libraries and the .NET Portable Class Library profiles.

If Mono is updated from within Xamarin Studio using **Check for updates** then the MDK should be installed.

## Windows - Installing Portable Class Libraries

Xamarin Studio on Windows uses Microsoft's .NET Framework instead of Mono so the Portable Class Libraries need to be installed separately. To install the Portable Class Libraries on Windows you have three options:

  1. Install Visual Studio 2013 (full or [Express version](http://www.microsoft.com/en-us/download/details.aspx?id=43733)). Update 2 or above is required.
  2. Install the [Portable Library Tools](https://visualstudiogallery.msdn.microsoft.com/b0e0b5e9-e138-410b-ad10-00cb3caf4981/) and the [Portable Library Reference Assemblies 4.6](http://www.microsoft.com/en-us/download/details.aspx?id=40727).
  3. Install the [Portable Library Tools](https://visualstudiogallery.msdn.microsoft.com/b0e0b5e9-e138-410b-ad10-00cb3caf4981/) and copy the .NETPortable directory from Mono over to Windows.

If you do not want to install Visual Studio 2013 then you should look at options 2 or 3.

One problem with option 2 is that not all the .NET Portable profiles, such as Profile 259, will be installed. The full list of what .NET Portable profiles are installed by each of the installers listed above is available from the [.NET Portable Profiles page](/projects/NuGet/PortableProfiles/)

Let us take a look in more detail at the option 2 since this has a few manual steps.

**Windows - Installing the Portable Library Tools and the Portable Library Reference Assemblies 4.6**

Before you start make sure Xamarin Studio is not running.

Download the [Portable Library Tools](https://visualstudiogallery.msdn.microsoft.com/b0e0b5e9-e138-410b-ad10-00cb3caf4981/).

To install the Portable Library Tools open a command prompt where PortableLibraryTools.exe was downloaded and run:

    PortableLibraryTools /buildmachine

Download the [Portable Library Reference Assemblies 4.6](http://www.microsoft.com/en-us/download/details.aspx?id=40727) and run the NetFx_PortableLibraryReferenceAssemblies46.msi installer. This will install a PortableReferenceAssemblies.zip file into the directory:

    C:\Program Files (x86)\Microsoft .NET Portable Library Reference Assemblies 4.6

This PortableReferenceAssemblies.zip file contains three directories (4.0, 4.5 and 4.6) which need to be extracted and copied into the PCLs directory:

     C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\.NETPortable

The PortableReferenceAssemblies.zip file contains files which already exist in the above folder and you should replace the existing files with the new versions from the .zip file.

If you installed Xamarin Studio before you installed the Portable Class Libraries you will now need to reinstall the Xamarin PCL profiles. The Xamarin PCL profiles will only be installed if the Portable Class Libraries were already installed. The simplest way to do this is to find **Xamarin** in the Control Panel's **Programs and Features**, right click it and select **Repair**.

{% img /images/blog/InstallingPortableClassLibrariesForXamarinStudio/AddRemoveProgramsRepairXamarinInstall.png 'Repairing Xamarin in Programs and Features' 'Repairing Xamarin in Programs and Features' %}

This will add a set of Xamarin .xml files into the profiles that are compatible with the Xamarin frameworks. For example looking at the Profile78 directory we see:

    C:\Program Files (x86)\Reference Assemblies\Microsoft\Framework\.NETPortable\v4.5\Profile\Profile78\SupportedFrameworks

 * .NET Framework 4.5.xml
 * Windows 8.xml
 * Windows Phone Silverlight 8.xml
 * Xamarin.Android.xml
 * Xamarin.iOS.xml

Now you should have full support for Portable Class Libraries in Xamarin Studio.