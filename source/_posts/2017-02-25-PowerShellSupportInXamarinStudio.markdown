---
layout: post
title: "PowerShell Support in Xamarin Studio"
date: 2017-02-25 18:00
comments: true
categories: PowerShell Xamarin MonoDevelop
---

Xamarin Studio version 6.0 and later now have PowerShell editing and debugging support with a PowerShell addin. The PowerShell addin uses the [PowerShell Editor Services](https://github.com/PowerShell/PowerShellEditorServices) which is also used by the [PowerShell extension](https://github.com/PowerShell/vscode-powershell) for Visual Studio Code.

{% img /images/blog/PowerShellSupportInXamarinStudio/DebuggingPowerShellScriptInXamarinStudio.png 'Debugging a PowerShell script in Xamarin Studio' 'Debugging a PowerShell script in Xamarin Studio' %}

## Features

 * Code completion
 * Debugging support
   * Immediate window
   * Locals window
   * Watch window
 * Find references
 * PowerShell file template
 * Rename variable or method
 * Show API documentation
 * Syntax highlighting
 
## Requirements

Xamarin Studio 6.x or MonoDevelop 6.x.

[PowerShell 6](https://github.com/PowerShell/PowerShell) needs to be installed on Mac and on Linux.

PowerShell 3 and higher are supported on Windows.

## Code Completion

As you type in the text editor you will see code completion for PowerShell variables.

{% img /images/blog/PowerShellSupportInXamarinStudio/PowerShellVariableCodeCompletion.png 'PowerShell variable code completion' 'PowerShell variable code completion' %}

Code completion for PowerShell commands.

{% img /images/blog/PowerShellSupportInXamarinStudio/PowerShellCommandCodeCompletion.png 'PowerShell command code completion' 'PowerShell command code completion' %}

An overview of PowerShell command parameters when you press the space key after entering a PowerShell command.

{% img /images/blog/PowerShellSupportInXamarinStudio/PowerShellCommandTooltipCodeCompletion.png 'PowerShell command parameters overview code completion' 'PowerShell command parameters overview code completion' %}

Code completion for PowerShell command parameters.

{% img /images/blog/PowerShellSupportInXamarinStudio/PowerShellCommandParameterCodeCompletion.png 'PowerShell command parameter code completion' 'PowerShell command parameter code completion' %}

Hovering the mouse over a PowerShell command will show a tooltip with information about that command.

{% img /images/blog/PowerShellSupportInXamarinStudio/PowerShellCommandTooltip.png 'PowerShell command tooltip on hover' 'PowerShell command tooltip on hover' %}

## Syntax Errors

Syntax errors are highlighted in the text editor. Hovering the mouse over the highlighted error will show information about the error.

{% img /images/blog/PowerShellSupportInXamarinStudio/PowerShellSyntaxError.png 'PowerShell syntax error highlighting' 'PowerShell syntax error highlighting' %}

## Find references

To find references of a variable or a method you can right click in the text editor and select Find References.

{% img /images/blog/PowerShellSupportInXamarinStudio/PowerShellFindReferencesContextMenu.png 'PowerShell find references text editor context menu' 'PowerShell find references text editor context menu' %}

The references found are then displayed in the Search Results window. 

{% img /images/blog/PowerShellSupportInXamarinStudio/PowerShellFindReferencesSearchResults.png 'PowerShell find references text editor context menu' 'PowerShell find references text editor context menu' %}

## Rename

A variable or method can be renamed in the text editor by right clicking and selecting Rename.

{% img /images/blog/PowerShellSupportInXamarinStudio/PowerShellRenameContextMenu.png 'PowerShell rename text editor context menu' 'PowerShell rename text editor context menu' %}

On typing the new name and the text will be replaced.

## Show API Documentation

Right clicking on a PowerShell command and selecting Show API Documentation will open the online help for that PowerShell command, if it is available, in the web browser.

{% img /images/blog/PowerShellSupportInXamarinStudio/PowerShellAPIDocumentationContextMenu.png 'API Documentation text editor context menu' 'API Documentation text editor context menu' %}

{% img /images/blog/PowerShellSupportInXamarinStudio/PowerShellAPIOnlineDocumentation.png 'PowerShell API documentation web page' 'PowerShell API documentation web page' %}

## Creating a new PowerShell Script

To create a new PowerShell script there is an empty PowerShell file template available from the New File dialog.

{% img /images/blog/PowerShellSupportInXamarinStudio/PowerShellFileTemplateInNewFileDialog.png 'PowerShell script file template in New File dialog' 'PowerShell script file template in New File dialog' %}

After creating a new PowerShell file it must be saved before it can be run or debugged.

## Debugging

To debug the currently active PowerShell file open in the text editor,
set a breakpoint on a line by clicking in the left hand margin, then select Start Debugging from the Run menu.

{% img /images/blog/PowerShellSupportInXamarinStudio/RunStartDebuggingMenu.png 'Run - Start Debugging menu' 'Run - Start Debugging menu' %}

Alternatively you can click the Run button in the main toolbar.

{% img /images/blog/PowerShellSupportInXamarinStudio/DebugRunMainToolbarButton.png 'Run button in the main toolbar' 'Run button in the main toolbar' %}

A solution does not need to be open in order for a PowerShell script to be run with the debugger. You can open just a PowerShell script into Xamarin Studio and then run the debugger.

Once the debugger has started you can select Step Over, Step In, Step Out or Continue Debugging from the Run menu by clicking one of the main toolbar buttons.

{% img /images/blog/PowerShellSupportInXamarinStudio/RunStepMenuItems.png 'Run menu with Step menu items' 'Run menu with Step menu items' %}

Hovering the mouse over a variable will open a tooltip showing the variable value.

{% img /images/blog/PowerShellSupportInXamarinStudio/DebugTooltipOnHover.png 'Debug tooltip on hover' 'Debug tooltip on hover' %}

## Breakpoints

Breakpoint conditions should use PowerShell syntax and not C# syntax. The Edit Breakpoint dialog says to use a C# boolean expression which is incorrect.

{% img /images/blog/PowerShellSupportInXamarinStudio/EditBreakpointDialog.png 'PowerShell breakpoint condition in Edit Breakpoint dialog' 'PowerShell breakpoint condition in Edit Breakpoint dialog' %}

Hit conditions are only partially supported. The PowerShell Editor Services debugger supports the 'When hit count is equal to'. Due to this restriction the other hit count options may not work as expected.

Printing a message and continuing is not currently supported.

Breaking when the value of an expression changes is not currently supported. 

Function and exception breakpoints are not currently supported.

## Locals Window

When debugging the Locals window will show the values of variables grouped by each PowerShell scope - Auto, Local, Script and Global.

{% img /images/blog/PowerShellSupportInXamarinStudio/LocalsWindow.png 'Locals Window' 'Locals Window' %}

## Watch Window

Variables and expressions can be entered in the Watch Window.

{% img /images/blog/PowerShellSupportInXamarinStudio/WatchWindow.png 'Watch window' 'Watch window' %}

Please note that entering a PowerShell command with missing parameters will cause the debugger to stop working. The PowerShell file will need to be closed and re-opened before the debugger will work again.

## Immediate Window

Expressions and variables can be entered in the immediate window to get or set values.

{% img /images/blog/PowerShellSupportInXamarinStudio/ImmediateWindow.png 'Immediate window' 'Immediate window' %}

As with the Watch Window, entering a PowerShell command with missing parameters will cause the debugger to stop working.

# Passing Arguments when Debugging

To pass arguments when debugging a PowerShell script you can select Debug PowerShell Script... from the Run menu.

{% img /images/blog/PowerShellSupportInXamarinStudio/DebugPowerShellScriptMenu.png 'Run - Debug PowerShell Script menu' 'Run - Debug PowerShell Script menu' %}

This will open a Debug PowerShell Script dialog where arguments can be specified. These arguments will be passed to the PowerShell script being run with the debugger. The settings used in this dialog will be remembered for the active text editor whilst it is open in Xamarin Studio.

{% img /images/blog/PowerShellSupportInXamarinStudio/DebugPowerShellScriptDialog.png 'Debug PowerShell Script dialog' 'Debug PowerShell Script dialog' %}

## Launch Configuration Support

In Visual Studio Code a launch.json file can be used to store launch configurations. These are supported in by the PowerShell addin in Xamarin Studio. The PowerShell addin will look for a launch.json file in the directory where the PowerShell file exists or in a .vscode subdirectory.  

The launch configurations are shown under Active Configurations in the Run menu. Only PowerShell launch configurations which have a request type of "launch" are supported.

{% img /images/blog/PowerShellSupportInXamarinStudio/PowerShellLaunchConfigurations.png 'PowerShell launch configurations in Run menu' 'PowerShell launch configurations in Run menu' %}

The currently selected launch configuration will be used when debugging or running the PowerShell script. By default no launch configuration will be selected. 

## Running without the Debugger

To run the PowerShell file with PowerShell directly, instead of using the debugger, select Start without Debugging from the Run menu.

{% img /images/blog/PowerShellSupportInXamarinStudio/RunStartWithoutDebuggingMenu.png 'Run - Start without Debugging menu' 'Run - Start without Debugging menu' %}

Output from the PowerShell script will be displayed in the Application Output window.

{% img /images/blog/PowerShellSupportInXamarinStudio/PowerShellScriptApplicationOutput.png 'PowerShell script Application Output window' 'PowerShell script Application Output window' %}

## Installation

The PowerShell addin is available from the [MonoDevelop addin repository](http://addins.monodevelop.com/) on the beta channel. To install the addin open the Add-in Manager, search for the PowerShell addin, then click the Install button.

{% img /images/blog/PowerShellSupportInXamarinStudio/AddinManagerDialogWithPowerShellAddin.png 'PowerShell addin in Addin manager dialog' 'PowerShell addin in Addin manager dialog' %}

## Source Code

 - [PowerShell addin for Xamarin Studio and MonoDevelop](https://github.com/mrward/monodevelop-powershell-addin)
 - [PowerShell Editor Services](https://github.com/PowerShell/PowerShellEditorServices)
