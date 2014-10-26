---
layout: post
title: "Xamarin Components and NuGet"
date: 2014-10-26 12:00
comments: true
categories: NuGet Xamarin
---

[Xamarin Studio 5.5](http://lastexitcode.com/blog/2014/10/19/NuGetSupportInXamarinStudio5-5/) and [Xamarin for Visual Studio 3.7](http://developer.xamarin.com/releases/vs/xamarin.vs_3/xamarin.vs_3.7/) both have support for [Xamarin Components](https://components.xamarin.com/) that have NuGet package dependencies. So you can now have Xamarin Studio or Visual Studio add NuGet packages to a project when a Component is installed from [Xamarin's Component Store](https://components.xamarin.com/).

The [NuGet Support in Xamarin Studio 5.5 post](http://lastexitcode.com/blog/2014/10/19/NuGetSupportInXamarinStudio5-5/) looked at installing a Component with a NuGet package dependency into a project but did not cover how to create one of these Components. So let us take a look at how to create a Component so it has NuGet package dependencies. 

In the following sections we will look at:

  * Adding a NuGet package dependency to an existing Component.
  * How to create a Component with NuGet package dependencies from scratch.
  * How to test a Component with NuGet package dependencies.

## Adding a NuGet Package Dependency to a Component

In this section we will look at modifying a Component so when it is installed it adds NuGet packages to the project as well as referencing the assemblies that are included in the Component.

First download the latest version of [xamarin-component command line application](https://components.xamarin.com/submit/xpkg) which has been updated to support Component's with NuGet packages dependencies.

The downloaded xpkg file is a zip file containing a xamarin-component.exe. So rename the file to have a .zip fo;e extension and then extract the executable.

So let us add a single NuGet package dependency to the Component. If you are using a component.yaml file to generate your component you can add the NuGet package dependency as follows:

    packages:
      "": Newtonsoft.Json, Version=5.0.8

With the above defined in your component.yaml file you can run the package command to generate your Component's .xam file.

Windows:

    xamarin-component package path/to/yaml-file-directory
    
Mac:

    mono xamarin-component package path/to/yaml-file-directory

If you then look inside your generated .xam file, which you can do by renaming its file extension to .zip, you will see the packages defined in the component/Manifest.xml file:

	<packages>
	  <package id="Newtonsoft.Json" version="5.0.8" />
	</packages>

If you are using the **create-manually** command line you can add the same NuGet package dependency by adding the following command line argument:

	--package="":"Newtonsoft.Json, Version=5.0.8"

This Component will install the Newtonsoft.Json NuGet package into any Android, iOS and Windows Phone project. If you need to install a particular NuGet package into a particular project type you can specify the target project type in the same way you do for assemblies in the Component's **lib** directory.

Shown below is a more complicated example from a component.yaml file where NUnit and Newtonsoft.Json are only installed into Android projects, whilst log4net is only installed into a iOS projects, and finally Ninject will be installed into all project types. Note that **mobile** used here is equivalent to using the empty string **""** which was used in the previous example with just the one NuGet package.

    packages:
      android:
        - NUnit, Version=2.6.2
        - Newtonsoft.Json, Version=5.0.8
      mobile: Ninject, Version=3.2.0
      ios: log4net, Version=2.0.0

If you then generate the Component again using the xamarin-component package command you will see the Component's manifest file now contains the following section:

	  <packages>
	    <package id="NUnit" version="2.6.2" framework="android" />
	    <package id="Newtonsoft.Json" version="5.0.8" framework="android" />
	    <package id="log4net" version="2.0.0" framework="ios" />
	    <package id="Ninject" version="3.2.0" framework="mobile" />
	  </packages>

If you are using the create-manually command line you can add the same NuGet package dependencies to your Component by adding the following command line arguments:

	--package="android":"NUnit, Version=2.6.2"
	--package="android":"Newtonsoft.Json, Version=5.0.8"
	--package="mobile":"Ninject, Version=3.2.0"
	--package="ios":"log4net, Version=2.0.0"
    
When installing the Component above into a project the NuGet package will be installed and a reference will be added to the assemblies in the **lib** directory of the Component. If you want to only add the NuGet package to the project, when the Component is installed, and not the assemblies in the lib directory then you can create a shell component which we will look at in the following section.

## Creating a Shell Component

A Shell Component is special type Component that is basically a wrapper around a NuGet package. It will only install the NuGet package into the project and not add references to any assemblies in the Component's **lib** directory.

To configure a component to be a shell component you can add the following to your component.yaml file:

    is_shell: true

When you generate your Component's .xam file you will see that the Component's Manifest.xml file now contains the is-shell attribute in the root component element:

	<component format="1" id="mycomponent" is-shell="true">

If you are using the xamarin-component's create-manually command line argument you can do the same thing by passing the following extra argument:

    --is-shell

Backwards compatibility is something to consider if you decide to create a Shell Component.  If you need a Shell Component to work with older versions of Xamarin Studio and Visual Studio that do not support Component's with NuGet package dependencies then you should also include the assemblies in the lib directory of the Component.  The [Android Support Library v13 Component](https://components.xamarin.com/view/xamandroidsupportv13-18) is one example that has a NuGet package dependency and also includes assemblies in its lib/android directory so it can be used in older versions of Xamarin Studio and Xamarin for Visual Studio.

## NuGet Package Sources

Note that the NuGet package must be available from the official NuGet package source on http://nuget.org before your Component is submitted to the Component Store.

 If you are testing the NuGet package that is not available from nuget.org then copy it into the [local machine's NuGet cache directory](http://lastexitcode.com/projects/NuGet/FileLocations/). Xamarin Studio and Visual Studio should find the NuGet package in the local NuGet cache instead of trying to get it from the online package source.

## Creating a Component from scratch

To generate files for a Component you can run xamarin-component.exe with the create argument and specify the directory you want the files to be created.

Windows:

    xamarin-component.exe create MyComponent
     
Mac:
   
    mono xamarin-component.exe create MyComponent

Running the above will prompt you to answer some information about your Component.

	INFO (create): Creating component files in MyComponent\component
	INFO (create):   * [NEW] MyComponent\component\Details.md
	INFO (create):   * [NEW] MyComponent\component\GettingStarted.md
	INFO (create):   * [NEW] MyComponent\component\License.md
	INFO (create):   * [NEW] MyComponent\component\icon-128.png
	INFO (create):   * [NEW] MyComponent\component\icon-512.png
	Your component needs a great name. What do you want to call it? MyComponent
	We need a short way to identify your component. Should we use the directory name
	 (Y/n)? y
	What's the version of your component? 1.0
	Who's the publisher of your component? MattWard
	What's the publisher's website? http://lastexitcode.com
	Give us a one-sentence summary of your component: My Component with NuGet Dependencies
	INFO (create):   * [NEW] MyComponent\component\component.yaml
	INFO (create): Component files created. Please edit MyComponent\component\component.yaml to configure your component for packaging, then run `xamarin-component	upload` to build, package, and upload your component to the Xamarin component store.

This will generate a set of files in the component subdirectory which will be the starting point for creating a Component with a NuGet package dependency.

For more detailed information on how to create a Component please see the [Submitting Components](http://developer.xamarin.com/guides/cross-platform/advanced/submitting_components/) page.

## Testing a Component



