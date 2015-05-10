---
layout: post
title: "Xamarin Components Directory Configuration"
date: 2015-05-10 11:00
comments: true
categories: Xamarin
---

One of the new features introduced in [Xamarin Studio 5.9](http://developer.xamarin.com/releases/studio/xamarin.studio_5.9/xamarin.studio_5.9/) is the ability to configure the directory where Xamarin Components are installed to when they are added to a project.

By default, when a Component from the [Xamarin Component store](https://components.xamarin.com/) is added to a project, the Component is installed to a Components directory inside the solution's directory.

{% img /images/blog/XamarinComponentsDirectoryConfiguration/DefaultXamarinComponentsDirectoryForSolution.png 'Default Components directory for a solution' 'Default Components directory for a solution' '' %}

The project will have references added that refer to assemblies inside this Components directory.

    <Reference Include="Microsoft.WindowsAzure.Mobile.Ext">
      <HintPath>..\Components\azure-mobile-services-1.3.1\lib\android\Microsoft.WindowsAzure.Mobile.Ext.dll</HintPath>
    </Reference>
    <Reference Include="Microsoft.WindowsAzure.Mobile">
      <HintPath>..\Components\azure-mobile-services-1.3.1\lib\android\Microsoft.WindowsAzure.Mobile.dll</HintPath>
    </Reference>
    <Reference Include="Newtonsoft.Json">
      <HintPath>..\Components\azure-mobile-services-1.3.1\lib\android\Newtonsoft.Json.dll</HintPath>
    </Reference>
    <Reference Include="System.Net.Http.Extensions">
      <HintPath>..\Components\azure-mobile-services-1.3.1\lib\android\System.Net.Http.Extensions.dll</HintPath>
    </Reference>
    <Reference Include="System.Net.Http.Primitives">
      <HintPath>..\Components\azure-mobile-services-1.3.1\lib\android\System.Net.Http.Primitives.dll</HintPath>
    </Reference>

If a project is shared between multiple solutions then Xamarin Studio can have multiple different Components directories, one for each solution. This can cause Xamarin Studio to modify the hint paths in the project file to use a different Components directory depending on which solution was opened.

A simple way to reproduce this problem is to create one solution with a project that has a Component, then create another solution in a different directory, and add the same project to this new solution. The Component will be downloaded again into the Components directory relative to the new solution and the assembly references in the project file will be modified to use this new Components location.

Now let us take a look at how to solve this problem by configuring the Components directory.

## Configuring the Components Directory

To configure the Components directory used by a project you can use a components.config file, as shown below.

  <components>
    <config>
      <add key="cachePath" value="..\Components" />
    </config>
  </components>

The path specified in the components.config file can be a full path or a relative path. If it is a relative path then it is relative to the directory containing the components.config file.

The path in the components.config file will be normalized so it contains the correct directory separators on non-Windows operating systems, so you can use either a forward slash or a backslash in the path.

Now let us take a look at how Xamarin Studio finds this components.config file.

Xamarin Studio, when a solution is opened, will check for a components.config file in several locations based on the solution's directory. If we have a solution in the directory /Users/matt/Projects/MyAndroidApp/ then the full set of locations checked is as follows:

  1. /Users/matt/Projects/MyAndroidApp/.components/components.config
  2. /Users/matt/Projects/MyAndroidApp/components.config
  3. /Users/matt/Projects/components.config
  4. /Users/matt/components.config
  5. /Users/components.config
  6. /components.config
  7. ~/Library/Preferences/Xamarin/Components/components.config
  
Note that on Windows the last location checked is:

  %AppData%\Xamarin\Components\components.config

If you put the components.config file in a directory that is a parent of multiple solutions then all the solutions can use this common components.config file.

If the components.config file is missing or cannot be read then the default Components directory is used, which is inside the solution's directory.

If there is an error whilst reading the components.config file then the error will be logged by Xamarin Studio and the default Components directory will be used.

The Components directory to be used is cached when the solution is loaded so changes made to the components.config file require the solution to be closed and re-opened before Xamarin Studio will use the new settings.

To help diagnose problems when configuring the Components directory Xamarin Studio will log information in the Components.log file. The Components.log file can be found by selecting Open Log Directory from Xamarin Studio's Help menu. Two examples taken from the Components.log file are shown below. The first example shows the message logged when a components.config file cannot be found.

    [2015-05-10 11:00:29.0] DEBUG: No components.config file found. Using default path. Files checked: /Users/matt/Projects/MyAndroidApp/.components/components.config
    /Users/matt/Projects/MyAndroidApp/components.config
    /Users/matt/Projects/components.config
    /Users/matt/components.config
    /Users/components.config
    /components.config
    /Users/matt/Library/Preferences/Xamarin/Components/components.config
    
The next example shows the message logged when a components.config file is found.

    [2015-05-10 11:10:24.1] DEBUG: Using custom components cache path '/Users/matt/Projects/MyAndroidApp/Components'. components.config file found at '/Users/matt/Projects/MyAndroidApp/components.config'.

## Component Restore

The latest version of [xamarin-component.exe](https://components.xamarin.com/submit/xpkg) also supports using the configured Components directory. Its restore command will restore the Components to the directory as specified in the components.config file.

    mono xamarin-component.exe restore path/to/solution.sln
    
xamarin-component.exe will look for the components.config file in the same directories as Xamarin Studio.

## Comparison with NuGet

NuGet has similar behaviour to Components in Xamarin Studio. All NuGet packages are downloaded to a packages directory inside the solution directory by default. To override this behaviour you can create a [NuGet.Config file](https://docs.nuget.org/consume/nuget-config-file). The NuGet.Config file allows the packages directory to be configured through a repositoryPath setting.

  <configuration>
    <config>
      <add key="repositoryPath" value="../../packages" />
    </config>
  </configuration>

NuGet will look for this NuGet.Config file in several places. Assuming the solution directory is /Users/matt/Projects/MyAndroidApp/ the NuGet.Config file will be looked for in the locations as shown below:

  1. /Users/matt/Projects/MyAndroidApp/.nuget/NuGet.Config
  2. /Users/matt/Projects/MyAndroidApp/NuGet.Config
  3. /Users/matt/Projects/NuGet.Config
  4. /Users/matt/NuGet.Config
  5. /Users/NuGet.Config
  6. /NuGet.config
  7. ~/.config/NuGet/NuGet.Config (Windows: %AppData%\NuGet\NuGet.Config)
