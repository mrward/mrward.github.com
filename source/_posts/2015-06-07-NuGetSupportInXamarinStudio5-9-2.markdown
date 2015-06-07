---
layout: post
title: "NuGet Support in Xamarin Studio 5.9.2"
date: 2015-06-07 10:00
comments: true
categories: NuGet Xamarin MonoDevelop
---

## Changes

   * NuGet 2.8.5 support
   * NuGet warning and error messages in Status Bar

More information on all the changes in Xamarin Studio 5.9.2 can be found in the [release notes](http://developer.xamarin.com/releases/studio/xamarin.studio_5.9/xamarin.studio_5.9/).

## NuGet 2.8.5 support

Xamarin Studio now supports NuGet 2.8.5.

NuGet 2.8.5 adds support for three new .NET target frameworks: DNX, DNXCore and Core.

With NuGet 2.8.5 supported you can now install the [latest pre-release version of xUnit](https://www.nuget.org/packages/xunit/2.1.0-beta2-build2981).

## NuGet warning and error messages in Status Bar.

Xamarin Studio 5.9 has a new native Status Bar on the Mac. This new Status Bar has a smaller width so the NuGet warning and error messages could be too long to be displayed. The screenshots below show a NuGet warning and error message in Xamarin Studio 5.9 that do not fit in the Status Bar.

{% img /images/blog/NuGetSupportInXamarinStudio5-9-2/NuGetWarningMessageTruncatedInStatusBar.png 'NuGet warning message truncated in status bar' 'NuGet warning message truncated in status bar' %}

{% img /images/blog/NuGetSupportInXamarinStudio5-9-2/NuGetErrorMessageTruncatedInStatusBar.png 'NuGet error message truncated in status bar' 'NuGet error message truncated in status bar' %}

In Xamarin Studio 5.9.2 the NuGet Status Bar messages have been shortened so they can be displayed in the new Status Bar without being truncated. The screenshots below show the new format of the NuGet warning and error messages shown in the Status Bar.

{% img /images/blog/NuGetSupportInXamarinStudio5-9-2/NuGetShortenedWarningMessageInStatusBar.png 'Shortened NuGet warning message in status bar' 'Shortened NuGet warning message in status bar' %}

{% img /images/blog/NuGetSupportInXamarinStudio5-9-2/NuGetShortenedErrorMessageInStatusBar.png 'Shortened NuGet error message in status bar' 'Shortened NuGet error message in status bar' %}