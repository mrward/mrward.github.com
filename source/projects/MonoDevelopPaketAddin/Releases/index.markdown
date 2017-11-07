---
layout: page
title: "MonoDevelop Paket Addin Releases"
date: 2017-11-07 17:00
comments: true
sharing: true
footer: true
---
## Version 0.4

2017-11-07

* Update to paket 5.120.1
* Add new Paket file icons provided by Václav Vančura
* Fix code completion not working in paket.dependencies file
* Fix warnings when running paket.exe with deprecated command line arguments
* Fix errors running paket.exe not reported in Paket Console and status bar.

## Version 0.3

2016-12-10

* Support installing specific versions from the Add Packages dialog. The Add Packages dialog now has "Latest" as a version in the combo box. If Latest is selected then the NuGet package is added without a version. If a specific version is selected then the version will be added to the paket.dependencies file. For pre-release NuGet packages the version is used even if Latest is selected otherwise Paket will use the latest stable version.
* Update to NuGet v3 so the v3 package source is supported.
* Update the Add Packages dialog to what is currently used in MonoDevelop 6.1.
* Use package sources defined in NuGet.Config file. Previously only the main nuget.org v2 package source was available in the Add Packages dialog.
* Allow switching to the configured package sources from the Add Packages dialog.
* The Add Packages dialog now supports the light and dark theme.

## Version 0.2

2016-10-15

* Update to Paket 3.23.2

## Version 0.1

2015-06-09

 * First Release
 
