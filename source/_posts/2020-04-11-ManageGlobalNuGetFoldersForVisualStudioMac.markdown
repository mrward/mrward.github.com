---
layout: post
title: "Manage Global NuGet Folders for Visual Studio for Mac"
date: 2020-04-11 12:00
comments: true
categories: NuGet Xamarin VSMac MonoDevelop
---

NuGet supports configuring the location of the following
[global folders](https://docs.microsoft.com/en-us/nuget/consume-packages/managing-the-global-packages-and-cache-folders):

 - Global NuGet packages cache
   - ~/.nuget/packages
 - HTTP cache
   - ~/.local/share/NuGet/v3-cache
 - NuGet plugins ache
   - ~/.local/share/NuGet/plugins-cache

The above folder locations can be found by running the following command from Terminal.

    nuget locals all -list

Here we will take a look at how these can be configured for use by Visual Studio for Mac 8.5
when running on macOS Catalina.

## Configuring Global Packages Folder Only

If only the global packages folder needs to be configured then the simplest way is to
define the globalPackagesFolder folder in the ~/.config/NuGet/NuGet.Config file.

```
<configuration>
  <config>
    <add key="globalPackagesFolder" value="/Volumes/YourDrive/NuGet/.nuget/packages" />
  </config>
  <packageSources>
    <add key="nuget.org" value="https://api.nuget.org/v3/index.json" protocolVersion="3" />
  </packageSources>
</configuration>
```

Visual Studio for Mac will need to be restarted before it picks up any changes to the
NuGet.Config file.

## Configuring Global Folders with Environment Variables

Visual Studio for Mac will not use environment variables defined in the Bash profile ~/.bash_profile or
the Zsh profile ~/.zprofile unless Visual Studio for Mac is run from the Terminal.

    open -n "/Applications/Visual Studio.app"

When Visual Studio for Mac is run from the Dock, or from LaunchPad, it does not inherit
these environment variables.

Let us take a look at how we can define these NuGet environment
variables so that Visual Studio for Mac will use them without having to launch it
from the Terminal.

Create the file ~/Library/LaunchAgents/environment.plist containing the following:

```
<?xml version=“1.0” encoding=“UTF-8”?>
<!DOCTYPE plist PUBLIC “-//Apple//DTD PLIST 1.0//EN” “https://www.apple.com/DTDs/PropertyList-1.0.dtd”>
<plist version=“1.0”>
<dict>
<key>Label</key>
<string>my.startup</string>
<key>ProgramArguments</key>
<array>
<string>sh</string>
<string>-c</string>
<string>launchctl setenv NUGET_PACKAGES /Volumes/YourDrive/NuGet/.nuget/packages
launchctl setenv NUGET_HTTP_CACHE_PATH /Volumes/YourDrive/NuGet/.nuget/v3-cache
launchctl setenv NUGET_PLUGINS_CACHE_PATH /Volumes/YourDrive/NuGet/.nuget/plugins-cache</string>
</array>
<key>RunAtLoad</key>
<true/>
</dict>
</plist>
```

Replace the NuGet paths `/Volumes/YourDrive/NuGet/` defined above as required.

Restart macOS to have this Launch Agent run.

After restarting you can check the environment variables are being used by NuGet by
using the command line:

    nuget locals all -list

Now when Visual Studio for Mac is launched from the Dock, or from LaunchPad, it will use these
custom NuGet folders.

## More Information

[launchctl](https://en.wikipedia.org/wiki/Launchd#launchctl) setenv defines environment variables
globally so they are available to all macOS
applications launched with [launchd](https://en.wikipedia.org/wiki/Launchd).
