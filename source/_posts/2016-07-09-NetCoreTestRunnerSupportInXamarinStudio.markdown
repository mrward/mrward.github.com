---
layout: post
title: ".NET Core Test Runner Support in Xamarin Studio"
date: 2016-07-09 12:00
comments: true
categories: NETCore Xamarin MonoDevelop
---

The latest version of the .NET Core addin for Xamarin Studio and MonoDevelop now supports [.NET Core Test Runners](https://docs.microsoft.com/en-us/dotnet/articles/core/testing/unit-testing-with-dotnet-test).

{% img /images/blog/NetCoreTestRunnerSupportInXamarinStudio/DotNetCoreTestRunnerSupportInXamarinStudio.png '.NET Core Test Runner in Xamarin Studio' '.NET Core Test Runner in Xamarin Studio' %}

Xamarin Studio uses the [.NET Core test communication protocol](https://docs.microsoft.com/en-us/dotnet/articles/core/tools/test-protocol) to support .NET Core test runners. This protocol provides a way to discover and run the unit tests provided by a .NET Core test runner.

When Xamarin Studio finds a testRunner in the project.json file it will attempt to discover the unit tests for that project.

    {
        "version": "1.1.0-*",
 
        "testRunner": "nunit",
    
        "dependencies": {,
            "NUnit": "3.4.0",
            "dotnet-test-nunit": "3.4.0-beta-1"
        },
    
        "frameworks": {
            "netcoreapp1.0": {
                "imports": [
                    "netcoreapp1.0",
                    "portable-net45+win8"
                ],
                "dependencies": {
                    "Microsoft.NETCore.App": {
                        "version": "1.0.0-*",
                        "type": "platform"
                    }
                }
            }
        }
    }

The discovered tests are then shown in the Unit Tests window.

{% img /images/blog/NetCoreTestRunnerSupportInXamarinStudio/DotNetCoreDiscoveredUnitTestsInUnitTestsWindow.png 'Discovered NET Core unit tests in Unit Tests window' 'Discovered NET Core unit tests in Unit Tests window' %}

After building the project the Unit Tests window will discover any new tests that have been added.

Tests can be run by clicking Run All or by right clicking a test in the Unit Tests window and selecting Run Test.

{% img /images/blog/NetCoreTestRunnerSupportInXamarinStudio/RunDotNetCoreTestsContextMenu.png 'Run NET Core unit tests in Unit Tests window' 'Run NET Core unit tests in Unit Tests window' %}

The test results are shown in the Test Results window.

{% img /images/blog/NetCoreTestRunnerSupportInXamarinStudio/DotNetCoreTestResultsWindow.png '.NET Core test results  in Test Results window' '.NET Core test results  in Test Results window' %}

Console output from the test runner is shown in the Application Output window.

{% img /images/blog/NetCoreTestRunnerSupportInXamarinStudio/DotNetCoreTestRunnerApplicationOutput.png '.NET Core Test Runner in Xamarin Studio' '.NET Core Test Runner in Xamarin Studio' %}

Debugging the unit tests is not yet supported.

[xUnit](https://xunit.github.io/) and [NUnit](http://nunit.org/) provide .NET Core test runners and both of these are supported in Xamarin Studio. More information can be found in existing tutorials on how to use these test runners:

   - [NUnit 3 Tests for .NET Core RC2 and ASP.NET Core RC2](http://www.alteridem.net/2016/06/18/nunit-3-testing-net-core-rc2/)
   - [Getting started with xUnit.net (.NET Core / ASP.NET Core)](http://xunit.github.io/docs/getting-started-dotnet-core.html)
