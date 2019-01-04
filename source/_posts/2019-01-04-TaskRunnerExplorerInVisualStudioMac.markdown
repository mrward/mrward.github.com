---
layout: post
title: "Task Runner Explorer in Visual Studio for Mac"
date: 2019-01-04 12:00
comments: true
categories: Xamarin MonoDevelop VSMac
---


The [Task Runner Explorer addin]([Task Runner Explorer for Visual Studio for Mac](https://github.com/mrward/monodevelop-language-server-addin)) provides a Task Runner Explorer
window, similar to the one in Visual Studio on Windows, which can be used to run tasks with
Cake, Gulp, Grunt, NPM and TypeScript.

{% img /images/blog/TaskRunnerExplorerInVisualStudioMac/TaskRunnerExplorerWindowGruntNpmTasks.png 'Task Runner Explorer window' 'Task Runner Explorer Window' %}

## Features

 - Task Runner Explorer Window
   - View tasks
   - Run tasks
   - View task output
   - Run and cancel long running tasks, such as tcs watch
   - Configure tasks to run when specific IDE events occur
     - Before Build
     - After Build
     - Clean
     - Project Opened
     - Solution Opened
 - Task Runners
   - Cake
   - Gulp
   - Grunt
   - NPM
   - TypeScript

## Supports

 - Visual Studio Mac 7.5 or later.

## Task Runner Explorer

To open the Task Runner Explorer window, from the View menu select Pads, then
select Task Runner Explorer.

{% img /images/blog/TaskRunnerExplorerInVisualStudioMac/ViewTaskRunnerExplorerMenu.png 'View Task Runner Explorer menu' 'View Task Runner Explorer menu' %}

The Task Runner Explorer will look for files supported by a task runner in the
solution directory and the project directory. It will also look at all files
that have been added to a project or that have been added to a solution folder.

{% img /images/blog/TaskRunnerExplorerInVisualStudioMac/TaskRunnerExplorerTypeScriptNpmTasks.png 'Task Runner Explorer TypeScript and NPM tasks' 'Task Runner Explorer TypeScript and NPM tasks' %}

The top left of the Task Runner Explorer shows a list of projects
or the solution that have tasks available. This can be used to filter the tasks shown in the
Task Runner Explorer window.

Currently changes made to tasks will not be detected automatically. To refresh
the task information you can click the Refresh button in the Task Runner Explorer
window.

{% img /images/blog/TaskRunnerExplorerInVisualStudioMac/TaskRunnerExplorerRefreshButton.png 'Task Runner Explorer Refresh button' 'Task Runner Explorer Refresh button' %}

### Running a Task

To run a task you can double click it or right click and select Run.

{% img /images/blog/TaskRunnerExplorerInVisualStudioMac/TaskRunnerExplorerRunMenu.png 'Task Runner Explorer Run menu' 'Task Runner Explorer Run menu' %}

Output from the task is shown on the right hand side of the Task Runner Explorer
window.

{% img /images/blog/TaskRunnerExplorerInVisualStudioMac/TaskRunnerNpmUpdateRunning.png 'NPM update running' 'NPM update running' %}

{% img /images/blog/TaskRunnerExplorerInVisualStudioMac/NpmUpdateRunCompleted.png 'NPM update completed' 'NPM update completed' %}

A long running task, such as tcs watch, will run until the solution is closed,
or the Stop button, availalble on the right hand side
of the Task Runner Explorer window, is clicked.

### Binding Tasks to IDE Events

Tasks can be configured to run when the following IDE events occur:

 - After Build
 - Before Build
 - Clean
 - Project or Solution Opened

If the task runner file is in a project directory then the build and clean
events are associated with the project. If the task runner file is in a solution directory
then the build events are associated with the solution.

To configure a task, right click it, select Bindings and then select the 
event.

{% img /images/blog/TaskRunnerExplorerInVisualStudioMac/TaskRunnerExplorerBindingsMenu.png 'Task Runner Explorer Bindings menu' 'Task Runner Explorer Bindings menu' %}

The binding will be displayed in the Bindings tab and will also be shown as
checked when the context menu for the task is opened.

{% img /images/blog/TaskRunnerExplorerInVisualStudioMac/TaskRunnerExplorerBeforeBuildBindingAdded.png 'Task Runner Explorer Before Build binding added' 'Task Runner Explorer Before Build binding added' %}

To remove the binding you can right click the task, select Bindings and
select the event again to uncheck it. Alternatively you can right click
it in the Bindings tab and select Remove.

The order in which the tasks are run for a particular IDE event can be changed
by right clicking the binding in the Bindings tab and selecting Move Up or Move Down.

{% img /images/blog/TaskRunnerExplorerInVisualStudioMac/TaskRunnerExplorerBindingsMoveDownMenu.png 'Task Runner Explorer binding Move Up/Down menus' 'Task Runner Explorer binding Move Up/Down menus' %}

The binding information is typically saved in a file in the same directory as the corresponding task
runner file, however this depends on how the task runner is implemented.

### Disabling Automatic Running of Tasks

In preferences there is a Task Runner Explorer section which shows
a check box that can be used to enable or disable the automatic
running of tasks on opening a project or solution, and when building or cleaning
the project.

{% img /images/blog/TaskRunnerExplorerInVisualStudioMac/TaskRunnerPreferences.png 'Preferences - Automatically run tasks option' 'Preferences - Automatically run tasks option' %}

## Cake Task Runner

The Cake task runner supports running tasks defined in a build.cake file.

{% img /images/blog/TaskRunnerExplorerInVisualStudioMac/CakeTaskRunner.png 'Cake Task Runner' 'Cake Task Runner' %}

This is based on the [Cake Task Runner for Visual Studio](https://github.com/cake-build/cake-vs). 

## Gulp Task Runner

The Gulp task runner supports running tasks defined in a gulpfile.js file.

{% img /images/blog/TaskRunnerExplorerInVisualStudioMac/GulpTaskRunner.png 'Gulp Task Runner' 'Gulp Task Runner' %}

[Gulp](https://gulpjs.com) needs to be installed separately.

## Grunt Task Runner

The Grunt task runner supports running tasks defined in a Gruntfile.js file.

{% img /images/blog/TaskRunnerExplorerInVisualStudioMac/GruntTaskRunner.png 'Grunt Task Runner' 'Grunt Task Runner' %}

[Grunt](https://gruntjs.com) needs to be installed separately.

## NPM Task Runner

The NPM task runner supports running tasks defined in a package.json file.

{% img /images/blog/TaskRunnerExplorerInVisualStudioMac/NpmTaskRunner.png 'NPM task runner' 'NPM task runner' %}

The NPM task runner is a port of [Mads Kristensen's](https://madskristensen.net/) 
[NPM Task Runner](https://github.com/madskristensen/NpmTaskRunner).

The NPM task runner supports running with the verbose NPM option defined. If a
task is selected then a button will be displayed on the left hand side of the
Task Runner Explorer window. If this is selected then npm will be passed the
`-d` argument when it is run.

{% img /images/blog/TaskRunnerExplorerInVisualStudioMac/NpmTaskRunnerVerboseButton.png 'NPM task runner Verbose button' 'NPM task runner Verbose button' %}

NPM needs to be installed separately.

## TypeScript Task Runner

The TypeScript task runner supports running tcs build and tcs watch if a tsconfig.json file
is found.

{% img /images/blog/TaskRunnerExplorerInVisualStudioMac/TypeScriptTaskRunner.png 'TypeScript task runner' 'TypeScript task runner' %}

{% img /images/blog/TaskRunnerExplorerInVisualStudioMac/TypeScriptWatchTaskOutput.png 'TypeScript tcs watch output' 'TypeScript tcs watch output' %}

The TypeScript task runner will try to use tsc that is distributed with the Web Tools extension that
is included in Visual Studio for Mac. If the Web Tools extension is not installed then the task
runner will fall back to running tsc directly.

## Installation

There are two addins to be installed:

 - [Task Runner](https://github.com/mrward/monodevelop-task-runner-addin/releases/download/0.1/MonoDevelop.TaskRunner_0.1.mpack)
 - [Task Runners Bundle](https://github.com/mrward/monodevelop-task-runner-addin/releases/download/0.1/MonoDevelop.TaskRunnersBundle_0.1.mpack)

The Task Runner is the main addin. This will be used by other task runner addins and provides the
main task runner API and services.

The Task Runners Bundle addin contains the Cake, Gulp, Grunt, NPM and TypeScript task runners. These
are currently included together as a single addin instead of being distributed separately.

Download both of the above .mpack files. Install the Task Runner addin first since the Task Runners Bundle
addin depends on it. To install an addin's .mpack file, open the Extensions Manager 
by selecting Extensions... from the main menu. Click the Install from file button. Select the .mpack file 
and then click the Open button. After installing both the addins restart Visual Studio for Mac.

The addin is not currently available from the main Visual Studio for Mac extensions server.

## Source Code

 - [Task Runner Explorer for Visual Studio for Mac](https://github.com/mrward/monodevelop-language-server-addin)
 - [Cake Task Runner for Visual Studio for Mac](https://github.com/mrward/monodevelop-cake-task-runner)
 - [Grunt Task Runner for Visual Studio for Mac](https://github.com/mrward/monodevelop-grunt-task-runner)
 - [Gulp Task Runner for Visual Studio for Mac](https://github.com/mrward/monodevelop-gulp-task-runner)
 - [NPM Task Runner for Visual Studio for Mac](https://github.com/mrward/NpmTaskRunner)
 - [TypeScript Task Runner for Visual Studio for Mac](https://github.com/mrward/monodevelop-typescript-task-runner)
