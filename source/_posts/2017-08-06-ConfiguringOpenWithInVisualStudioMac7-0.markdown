---
layout: post
title: "Configuring Open With in Visual Studio for Mac 7.0"
date: 2017-08-06 12:00
comments: true
categories: Xamarin MonoDevelop VSMac
---

Visual Studio on Windows can be [configured to use a different editor to open a file](https://msdn.microsoft.com/en-us/library/hy2sthf1.aspx). With Visual Studio for Mac, whilst you can open a file with a selection of editors and applications, it is not currently possible to change the default editor or add a custom application.

The [Open With addin](https://github.com/mrward/monodevelop-open-with-addin) provides an Open With dialog that allows you to change the default editor or application used to open a file in a similar way to how this is done in Visual Studio on Windows.

## Features

 - Supports changing the default editor used to open a file.
 - Supports adding a custom application to open a file.
 - Editor configuration is saved and will be available on restarting Visual Studio for Mac.

## Supports

 - Visual Studio Mac 7.0 or later.

## Configuring the Default Editor or Application

To configure the application or editor used to open file you can right click the file in the Solution window and select Open With - Preferences...

{% img /images/blog/ConfiguringOpenWithInVisualStudioMac7-0/OpenWithPreferencesMenu.png 'Open With - Preferences context menu' 'Open With - Preferences context menu' %}

This will open the Open With dialog.

{% img /images/blog/ConfiguringOpenWithInVisualStudioMac7-0/OpenWithDialog.png 'Open With dialog' 'Open With dialog' %}

The editors and applications that support opening the file are displayed. The default application is indicated by having (Default) next to its name.

To change the default application or editor used to open the file select it and then click the Set as Default button. Click OK to close the dialog and save the configuration.

If a file is currently open then it will need to be closed before it is opened in the new default editor or application.

## Adding a Custom Application

A custom application can be added to the list of applications shown in the Open With menu. First open the Open With dialog by selecting Open With - Preferences. Then click the Add button to open the Add Application dialog.

{% img /images/blog/ConfiguringOpenWithInVisualStudioMac7-0/AddApplicationDialog.png 'Add Application dialog' 'Add Application dialog' %}

The Browse button can be used to find an application.

The Friendly Name is the name that will be displayed in the Open With menu for the custom application.

Arguments cannot be specified if a Mac application (.app) is used. However if the application is not a Mac application, for example, it is a C# program, then arguments can be passed. To pass the filename to the program you can use {0} in the Arguments text box. This placeholder will be expanded to be the full filename path when the program is run.

## Removing a Custom Application

To remove a custom application for a file first open the Open With dialog by selecting Open With - Preferences. Select the application you want to remove. Then click the Remove button.

The Remove button is only enabled for custom applications that you have added. The built-in editors and applications cannot be removed.

## Installation

The Open With addin is available to download from [GitHub](https://github.com/mrward/monodevelop-open-with-addin/releases/download/0.1/MonoDevelop.OpenWith_0.1.mpack).

To install the addin open the Extensions Manager by selecting Extensions... from the main menu. Click the Install from file button. Select the .mpack file and then click the Open button.

The addin is also in the Extensions Manager from the 
[Visual Studio Extension Repository (Beta channel)](http://addins.monodevelop.com/Project/Index/306). It is not currently published to the main MonoDevelop addin server.

## Source Code

 - [Open With addin for Visual Studio for Mac](https://github.com/mrward/monodevelop-open-with-addin)
 
