---
layout: post
title: "NuGet Support in Xamarin Studio 5.4"
date: 2014-09-29 19:00
comments: true
categories: NuGet Xamarin MonoDevelop
---

## New Feature

   * Added support for the new Unified target frameworks for iOS and Mac:
   	   * Xamarin.iOS
	   * Xamarin.Mac

More details on all the new features and changes in Xamarin Studio 5.4 can be found in the [release notes](http://developer.xamarin.com/releases/studio/xamarin.studio_5.4/xamarin.studio_5.4/).

## New Unified Target Frameworks

Xamarin recently announced [a new Unified API for its iOS and Mac products](http://blog.xamarin.com/unified-api-with-64-bit-support-for-ios-and-mac/). This new Unified API makes it easier to share code between Mac and iOS as well as allowing you to support 32 and 64 bit applications with the same binary.

To use this new Unified API you can create a new Unified project for iOS or for Mac. There are several project templates available from Xamarin Studio's New Project Dialog.

{% img /images/blog/NuGetSupportInXamarinStudio5-4/UnifiedMacAndIOSProjectsInNewProjectDialog.png 'Unified iOS and Mac Projects in New Project Dialog' 'Unified iOS and Mac Projects in New Project Dialog' %}

These new Unified projects use the following new target frameworks:

  * Xamarin.iOS
  * Xamarin.Mac

## NuGet Support for the New Unified Target Frameworks

Introducing these two new frameworks meant that NuGet needed to be updated so it had support for them. The following changes were made to NuGet:
 
  * Add Xamarin.iOS and Xamarin.Mac as known frameworks.
  * Make frameworks that have a name that starts with **Xamarin** optional when checking the compatibility of Portable Class Libraries (PCLs) inside a NuGet package with a Portable Class Library project. 

With these new Unified frameworks being used by the projects, and being recognised as known frameworks by NuGet, you can now create a NuGet package with assemblies that specifically target these frameworks.  The following shows part of a .nuspec file with the assemblies compiled for each framework being copied into the appropriate target lib folder inside the NuGet package.

    <files>
		<file src="lib\Xamarin.iOS\*.*" target="lib\Xamarin.iOS10" />
		<file src="lib\Xamarin.Mac\*.*" target="lib\Xamarin.Mac20" />
	</files>

One example NuGet package that explicitly targets both of the new Unified frameworks is [Splat](https://www.nuget.org/packages/Splat/) which was created by Paul Betts.

Having the Xamarin frameworks treated as optional by NuGet allows a PCL project, on a machine with the Xamarin PCL profiles registered, to install a NuGet package with a PCL assembly that does not explicitly have the Xamarin frameworks registered as part of its portable profile.

Support for the new Xamarin frameworks will be available in the official NuGet from Microsoft in version 2.8.3. [NuGet 2.8.3](https://nuget.codeplex.com/releases/view/133091) is currently available as alpha release.

## Portable Class Libraries

In order to be able to install a NuGet package containing PCL assemblies, such as Json.NET, into Unified iOS or Unified Mac project you will need to have the PCL profile XML files for Unified iOS and Mac installed on your machine.

On the Mac you can get these new PCL profile XML files by installing Mono 3.10.0, which is currently available from Xamarin Studio on the alpha channel.

On Windows, there is a [separate installer](http://xvs.xamarin.com/Xamarin.iOS.PortableNuGet.msi) which will register the Unified iOS framework with the PCL profiles on your machine and also installs an alpha version of Microsoft's NuGet Package Manager (2.8.3) into Visual Studio.

