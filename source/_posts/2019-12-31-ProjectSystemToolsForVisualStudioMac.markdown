---
layout: post
title: "Project System Tools for Visual Studio for Mac"
date: 2019-12-31 12:00
comments: true
categories: Xamarin MonoDevelop VSMac
---

The [Project System Tools extension](https://github.com/mrward/monodevelop-project-system-tools) provides 
MSBuild design-time and build logging for Visual Studio for Mac.

{% img /images/blog/ProjectSystemToolsForVisualStudioMac/BuildLoggingBinLogFile.png 'Build Logging window and BinLog tree view' 'Build Logging window and BinLog tree view' %}

This is based on the [Project System Tools available for Visual Studio on Windows](https://github.com/dotnet/project-system-tools) and also re-uses code from this extension.

## Features

 - Build Logging window
   - Shows a list of builds and design-time builds
 - View MSBuild log output for all builds and design-time builds
 - View MSBuild binary logs for builds
   - Build tab shows a tree view of the build results from the binary log
   - Target Summary tab shows target name, number of calls, timings and file location
   - Task Summary tab shows task name, number of calls, timings and file location

## Supports

 - Visual Studio Mac 8.1 or later.

## Build Logging Window

To open the Build Logging Window

  - Select View - Pads - Build Logging

{% img /images/blog/ProjectSystemToolsForVisualStudioMac/BuildLoggingWindow.png 'Build Logging window' 'Build Logging window' %}

Click the green arrow to enable logging for builds and design-time builds.

To stop the logging click the red square.

To filter the targets use the combo box to restrict the items shown to builds or design time builds, or use the search on the right hand side of the window.

## MSBuild Log Output

To open the MSBuild log output

  - Double click the row in the Build Logging window or
  - Right click the row and select Open Log File

{% img /images/blog/ProjectSystemToolsForVisualStudioMac/BuildLoggingOpenLogFileMenu.png 'Open Log File context menu' 'Open Log File context menu' %}

{% img /images/blog/ProjectSystemToolsForVisualStudioMac/MSBuildLogFile.png 'MSBuild Log File' 'MSBuild Log File' %}

The verbosity of the MSBuild log output is configured in Preferences - Projects - Build - Log verbosity.

## MSBuild Binary Log

To open a binary log file

  - Right click the row in the Build Logging window and select Open Binary Log File

Note that binary logs are only available when the project or solution is built. 
Support for design-time build binary logs should be available in Visual Studio for Mac 8.5.

Three tabs are provided by the Project System Tools extension when a binary log file is opened.

  - Build
    - Tree view of the binary log targets and tasks
    - Properties window shows more information about the selected task or target
  - Target Summary
     - Target name
     - Source filename
     - Number of calls
     - Time taken
     - Percentage of total time taken
  - Task Summary
     - Task name
     - Source filename
     - Number of calls
     - Time taken
     - Percentage of total time taken

## Build Tab

{% img /images/blog/ProjectSystemToolsForVisualStudioMac/MSBuildBinaryLogFileBuildTab.png 'MSBuild Binary Log File - Build tab' 'MSBuild Binary Log File - Build tab' %}

The Build tab shows the run times of the targets and tasks, and whether they ran successfully.

Skipped tasks and targets are shown in light grey text.

Selecting a tree node in the Build tab will show more information about that node in the Properties window.

{% img /images/blog/ProjectSystemToolsForVisualStudioMac/BuildTabProperties.png 'MSBuild Binary Log File - Build tab - Properties window' 'MSBuild Binary Log File - Build tab - Properties window' %}

## Target Summary Tab

{% img /images/blog/ProjectSystemToolsForVisualStudioMac/TargetSummaryTab.png 'MSBuild Binary Log File - Target Summary tab' 'MSBuild Binary Log File - Target Summary tab' %}

Targets are shown in the order they were run and can be sorted by clicking the column headers.

## Task Summary Tab

{% img /images/blog/ProjectSystemToolsForVisualStudioMac/TaskSummaryTab.png 'MSBuild Binary Log File - Task Summary tab' 'MSBuild Binary Log File - Task Summary tab' %}

Tasks are shown in the order they were run and can be sorted by clicking the column headers.

## Project System Tools Installation

The Project System Tools extension is available from the Visual Studio for Mac extensions repository. To install the addin:

 - From the main menu, open the Extensions Manager dialog.
 - Select the Gallery tab.
 - Expand IDE extensions.
 - Select Project System Tools
 - Click the Refresh button if the extension is not visible.
 - Click Install… to install the extension.
 - Restart Visual Studio for Mac.

## Links

 - [Project System Tools source code - Visual Studio for Mac](https://github.com/mrward/monodevelop-project-system-tools)
 - [Project System Tools Marketplace - Visual Studio on Windows](https://marketplace.visualstudio.com/items?itemName=VisualStudioProductTeam.ProjectSystemTools)
 - [Project System Tools source code - Visual Studio for Windows](https://github.com/dotnet/project-system-tools)
