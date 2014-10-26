---
layout: post
title: "Xamarin Components and NuGet"
date: 2014-10-26 12:00
comments: true
categories: NuGet Xamarin
---

[Xamarin Studio 5.5](http://lastexitcode.com/blog/2014/10/19/NuGetSupportInXamarinStudio5-5/) and [Xamarin for Visual Studio 3.7](http://developer.xamarin.com/releases/vs/xamarin.vs_3/xamarin.vs_3.7/) have support for [Xamarin Components](https://components.xamarin.com/) with NuGet package dependencies. So you can now have Xamarin Studio or Visual Studio add NuGet packages to a project when a Component is installed from [Xamarin's Component Store](https://components.xamarin.com/).

The [NuGet Support in Xamarin Studio 5.5 post](http://lastexitcode.com/blog/2014/10/19/NuGetSupportInXamarinStudio5-5/) looked at installing a Component with a NuGet package dependency into a project but did not cover how to create one of these Components. So let us take a look at how to modify an existing Component so it has NuGet package dependencies. 

## Adding a NuGet Package Dependency to a Component

In this section we will look at modifying a Component so when it is installed it adds NuGet packages to the project as well as referencing the assemblies that are included in the Component.

First download the latest version of the [xamarin-component command line application](https://components.xamarin.com/submit/xpkg) which has been updated to support Component's with NuGet packages dependencies.

The downloaded xpkg file is a zip file containing xamarin-component.exe. So rename the file to have a .zip file extension and then extract the executable.

Now let us see how to add a single NuGet package dependency to the Component. If you are using a component.yaml file to generate your Component you can add the NuGet package dependency by adding the following to your component.yaml:

    packages:
      "": Newtonsoft.Json, Version=5.0.8

With the above defined in your component.yaml file you can run the xamarin-component.exe **package** command to generate your Component's .xam file.

Windows:

    xamarin-component.exe package path\to\directory-with-component-yaml
    
Mac:

    mono xamarin-component.exe package path/to/directory-with-component-yaml

If you then look inside your generated Component file (.xam), which you can do by renaming its file extension to .zip, you will see the package defined in the component/Manifest.xml file:

	<packages>
	  <package id="Newtonsoft.Json" version="5.0.8" />
	</packages>

If you are using the xamarin-component.exe **create-manually** command line you can add the same NuGet package dependency by adding the following command line argument:

	--package="":"Newtonsoft.Json, Version=5.0.8"

This packaged Component will install the Newtonsoft.Json NuGet package into any Android, iOS or Windows Phone project. If you need to install a particular NuGet package for a particular project type you can specify the target project type for the NuGet package dependency. Shown below is a more complicated example from a component.yaml file where NUnit and Newtonsoft.Json are configured so they will only be installed into Android projects, whilst log4net is only installed into iOS projects, and finally Ninject will be installed into all project types.

    packages:
      android:
        - NUnit, Version=2.6.2
        - Newtonsoft.Json, Version=5.0.8
      mobile: Ninject, Version=3.2.0
      ios: log4net, Version=2.0.0

Note that **mobile** used here is equivalent to the empty double quoted string "" which was used in the previous example with the single NuGet package.

If you then generate the Component again using the xamarin-component.exe package command you will see the Component's manifest file now contains the following:

	  <packages>
	    <package id="NUnit" version="2.6.2" framework="android" />
	    <package id="Newtonsoft.Json" version="5.0.8" framework="android" />
	    <package id="log4net" version="2.0.0" framework="ios" />
	    <package id="Ninject" version="3.2.0" framework="mobile" />
	  </packages>

If you are using the xamarin-component.exe create-manually command line you can add the same NuGet package dependencies to your Component by adding the following command line arguments:

	--package="android":"NUnit, Version=2.6.2"
	--package="android":"Newtonsoft.Json, Version=5.0.8"
	--package="mobile":"Ninject, Version=3.2.0"
	--package="ios":"log4net, Version=2.0.0"

With the NuGet package dependencies defined as shown in the previous examples when you install the Component into a project the NuGet package will be installed and a reference will be added to the assemblies in the **lib** directory of the Component. If you want to only add the NuGet package to the project and not the assemblies in the lib directory then you can create a Shell Component which we will look at in the following section.

## Creating a Shell Component

A Shell Component is special type Component that is basically a wrapper around one or more NuGet packages. It will only install the NuGet package into the project and not add references to any assemblies in the Component's **lib** directory.

To configure a component to be a Shell Component you can add the following to your component.yaml file:

    is_shell: true

When you generate your Component's .xam file you will see that the Component's Manifest.xml file now contains the **is-shell** attribute in the component element:

	<component format="1" id="mycomponent" is-shell="true">

If you are using the xamarin-component.exe create-manually command line argument you can do the same thing by passing the following argument:

    --is-shell

Backwards compatibility is something to consider if you decide to create a Shell Component. If you need a Shell Component to work with older versions of Xamarin Studio and Xamarin for Visual Studio that do not support Component's with NuGet package dependencies then you should also include the assemblies in the lib directory of the Component. The [Android Support Library v13 Component](https://components.xamarin.com/view/xamandroidsupportv13-18) is one example that has a NuGet package dependency and also includes an assembly in its lib/android directory. When installing the Android Support Library v13 Component into an older version of Xamarin Studio the NuGet package will not be installed and instead the assembly will be referenced from the Component's lib/android directory. If the Android Support Library v13 Component is installed with Xamarin Studio 5.5 or above then the NuGet package will be installed but the assembly from the lib/android directory will not be referenced.

## NuGet Package Sources

The NuGet package dependencies that a Component has must be available from the [official NuGet Gallery](https://nuget.org) before your Component is submitted to the Component Store.

If you are testing a NuGet package that is not currently available from the [official NuGet Gallery](https://nuget.org) then you can copy it into the [local machine's NuGet cache directory](http://lastexitcode.com/projects/NuGet/FileLocations/). Xamarin Studio and Xamarin for Visual Studio should find the NuGet package in the local NuGet cache instead of trying to download it from the NuGet Gallery.

## Creating a Component

There are some example Component's that have NuGet package dependencies available on my [GitHub page](https://github.com/mrward/xamarin-test-components). The [Awesome Component example](https://github.com/mrward/xamarin-test-components/tree/master/AwesomeComponent) is a Shell Component that uses a rakefile and the xamarin-component create-manually command line to generate the Component. The [My Component example](https://github.com/mrward/xamarin-test-components/tree/master/MyComponent) is another Shell Component that uses a component.yaml file that defines NuGet package dependencies.

For more detailed information on how to create a Component please see the [Submitting Components](http://developer.xamarin.com/guides/cross-platform/advanced/submitting_components/) page.




