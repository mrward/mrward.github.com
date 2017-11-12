---
layout: post
title: "Template Creator for Visual Studio for Mac"
date: 2017-11-12 14:30
comments: true
categories: Xamarin MonoDevelop VSMac Templating
---

The [Template Creator extension](https://github.com/mrward/monodevelop-template-creator-addin) provides a way to create a project template from an existing project
or solution open in Visual Studio for Mac and have the project template available in the New
Project dialog straight away.

The Template Creator uses the [.NET Core templating engine](https://github.com/dotnet/templating)
to create the projects from the project templates.

Let us take a look in more detail at what the Template Creator extension provides in Visual Studio for Mac.

## Features

   * Create a project template from an existing solution or project
   * Create custom top level project template categories

## Supports

 - Visual Studio Mac 7.0 or later.

## Creating a new Project Template

To create a new project template for a project opened in Visual Studio for Mac,
right click the project and select Create Template.

{% img /images/blog/TemplateCreatorForVisualStudioMac/ProjectCreateTemplateMenu.png 'Project - Create Template context menu' 'Project - Create Template context menu' %}

A Template Information dialog is then opened.

{% img /images/blog/TemplateCreatorForVisualStudioMac/TemplateInformationDialog.png 'Template Information dialog' 'Template Information dialog' %}

The information in the dialog is used to populate the generated template.json file. Further
details on what the template.json file holds is available in the [reference for template.json](https://github.com/dotnet/templating/wiki/Reference-for-template.json)].

 - Author
   - Author of the project template.
 - Display Name
   - This is the template name displayed in the New Project dialog.
 - Description
   - This is the description displayed in the New Project dialog.
 - Category
   - This is the category in the New Project dialog used by the template.
 - Short Name
   - This is the short name for the template that can be used from the .NET Core command line.
 - Default Project Name
   - This is the default project name that will be used with the .NET Core command line.
 - Identity
   - This is the unique id for the template. This should be unique across all custom
   project templates.
 - Group Identity
   - This is typically a substring of the template's Identity.

By default the project template will use the Other - .NET - General category in the
New Project dialog. To change the category click the browse button next to the category text
box to open the Template Categories dialog.

{% img /images/blog/TemplateCreatorForVisualStudioMac/TemplateCategoriesDialog.png 'Template Categories dialog' 'Template Categories dialog' %}

Select the required category and click OK. The selected category will then be updated
in the Template Information dialog.

Click OK to generate the template.json file. The template.json file will be opened
in the text editor. It will also be displayed in the Solution window in the
.template.config folder.

{% img /images/blog/TemplateCreatorForVisualStudioMac/ProjectTemplateJsonFile.png 'template.json file created for project' 'template.json file created for project' %}

The project template is now available in the New Project dialog.

{% img /images/blog/TemplateCreatorForVisualStudioMac/CustomProjectTemplateInNewProjectDialog.png 'Custom project template in New Project dialog' 'Custom template in New Project dialog' %}

## Updating a Project Template

After the project template is created you can modify the original project and its
template.json file as required.

Changes made to the project are available in the New Project dialog immediately.

Changes made to the template.json file are available immediately in the same
instance of Visual Studio for Mac where the template.json file is being edited.

## Multiple Projects in a Template

A project template may create more than one project. To create a project template
for all projects in the solution right click the solution and select Create Template.

{% img /images/blog/TemplateCreatorForVisualStudioMac/SolutionCreateTemplateMenu.png 'Solution - Create Template context menu' 'Solution - Create Template context menu' %}

A template.json file will be created in a .template.config directory inside the solution's
directory.

{% img /images/blog/TemplateCreatorForVisualStudioMac/TwoProjectsTemplateJsonFile.png 'template.json file created for soluton' 'template.json file created for solution' %}

Note that in order for the new projects created to use the name specified in the New Project
dialog they should all have a common start to their name. The template.json file's `sourceName`
will be the part that is replaced with the name of the project specified in the New Project dialog.

## Configuring Registered Project Templates

The .NET Core templating engine supports project templates in NuGet packages or project templates
that unpackaged in a directory. The Template Creator extension does
not package the project templates into a NuGet package and instead stores a set of directories that
are scanned by the .NET Core templating engine for templates. The set of directories that are
registered can be viewed and updated in the Preferences dialog from the Templates - Custom Folders section.

{% img /images/blog/TemplateCreatorForVisualStudioMac/PreferencesCustomFolders.png 'Preferences dialog - Templates - Custom Folders' 'Preferences dialog - Templates - Custom Folders' %}

The templating engine will scan the configured directories and look for all the template.json files
which define the project templates. The project templates found are then made available to the
Visual Studio for Mac's New Project dialog by the Template Creator extension.

When the template.json file is created using the information in the Template Information dialog
the Template Creator extenxion also registers the
project's directory so it will be scanned by the templating engine.

To remove a project
template from the New Project dialog you can either delete the template.json file or 
remove the folder from the Custom Folders defined in the Preferences dialog.

If you have an existing set of project templates that use template.json files and are not
in a NuGet package then you can register a single parent directory in the Preferences dialog.

## Custom Categories

Custom top level categories can be defined for your project templates by adding them in
Preferences - Templates - Custom Categories. 

{% img /images/blog/TemplateCreatorForVisualStudioMac/PreferencesCustomCategories.png 'Preferences dialog - Templates - Custom Categories' 'Preferences dialog - Templates - Custom Categories' %}

The Add Top Level Category button will add a new top level category and two child category
levels. The New Project dialog requires three category levels. The ids used should ensure that
the full path to the template's category is unique. For example, with a custom category
`custom/net/general` there should not be another top level `custom` category id, but the second
and third level category ids can be re-used in another top level category, such as `top/net/general`.

The Add Category button will add a single child category to the currently selected category
whilst the Remove button will remove the selected category.

Note that changes to the categories require Visual Studio for Mac to be restarted before they
are visible in the New Project dialog.

Also note that it is not currently possible to extend existing project template categories
using the Template Creator extension.

## Diagnosing Template Problems

If the template does not appear in the New Project dialog or fails to be created then
you may be able to diagnose the problem by opening the Templating Log window. This is
available from the View - Pads menu.

{% img /images/blog/TemplateCreatorForVisualStudioMac/MainMenuViewPadsTemplatingLog.png 'Main menu - View - Pads - Templating Log' 'Main menu - View - Templating Log' %}

This window will show messages returned from the .NET Core templating engine and any errors
reported by the Template Creator extension.

{% img /images/blog/TemplateCreatorForVisualStudioMac/TemplatingLogWindow.png 'Templating Log window' 'Templating Log window' %}

## Installation

The Template Creator extension is available to download from [GitHub](https://github.com/mrward/monodevelop-template-creator-addin/releases/download/0.1/MonoDevelop.TemplateCreator_0.1.mpack).

To install the extension open the Extensions Manager by selecting Extensions... from the main menu. Click the Install from file button. Select the .mpack file and then click the Open button.

The extension is also available from a [custom MonoDevelop 7.0 extension server](https://github.com/mrward/monodevelop-addins). It is not currently published to the main Visual Studio for Mac extension gallery.

## Source Code

 - [Template Creator extension for Visual Studio for Mac and MonoDevelop](https://github.com/mrward/monodevelop-template-creator-addin)

## Further Reading

 - [How to create your own templates for dotnet new](https://blogs.msdn.microsoft.com/dotnet/2017/04/02/how-to-create-your-own-templates-for-dotnet-new/)
 - [Reference for template.json](https://github.com/dotnet/templating/wiki/Reference-for-template.json)
 - [Creating Visual Studio project and solution templates - Part 3, VS for Mac extension](https://www.jimbobbennett.io/creating-visual-studio-project-and-solution-templates-part-3-vs-for-mac-extension/)
 - [SideWaffle v2 for Visual Studio](https://github.com/ligershark/sidewafflev2)