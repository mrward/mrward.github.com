---
layout: post
title: ".NET Core Support in Visual Studio for Mac 7.5"
date: 2018-05-20 09:00
comments: true
categories: NETCore Xamarin VSMac MonoDevelop
---

## New Features

  * .NET Core 2.1 SDK support
    * .NET Core 2.1 SDK project templates
    * HTTPS development certificate support
    * .NET Core global tools added to PATH on startup
  * .NET Core location can now be configured
  * Xamarin.Forms project templates now use .NET Standard projects
  * Performance improvements when using projects with many files
  * .NET Core templating engine has been updated
  * Improved support for Xamarin.Forms
  * Item templates are now supported with the .NET Core templating engine

More information on all the new features and changes in [Visual Studio for Mac 7.5](https://www.visualstudio.com/vs/visual-studio-mac/) can be found in the 
[release notes](https://docs.microsoft.com/en-us/visualstudio/releasenotes/vs2017-mac-relnotes#-visual-studio-2017-for-mac-version-75-release-notes).

## .NET Core 2.1 SDK support

Visual Studio for Mac 7.5 includes support for the .NET Core 2.1 SDK which is currently 
available as a [release candidate](https://blogs.msdn.microsoft.com/dotnet/2018/05/07/announcing-net-core-2-1-rc-1/). The following
sections will look into the support provided by Visual Studio for Mac 7.5 for the .NET Core 2.1 SDK.

### .NET Core 2.1 SDK project templates
    
If .NET Core 2.1.300 SDK is installed then the .NET Core 2.1 project templates
will be available in the New Project dialog.

{% img /images/blog/NetCoreSupportInVisualStudioMac7-5/NewProjectDialogNetCore21TargetFramework.png 'New Project dialog - .NET Core 2.1 target framework option' 'New Project dialog - .NET Core 2.1 target framework option' %}
  
The .NET Core 2.1 project templates are not included with Visual Studio for Mac and 
will be searched for in the 2.1.300 SDK templates directory.
    
    /usr/local/share/dotnet/sdk/2.1.300-rc1-008673/Templates/
    
The project templates for .NET Core 2.0 and 1.1 are currently still being shipped
with Visual Studio for Mac.

### HTTPS development certificate support

ASP.NET Core 2.1 projects use HTTPS by default. In order to be able to run
ASP.NET Core 2.1 projects with HTTPS a development certificate needs to be installed.
Visual Studio for Mac will detect if the development certificate is missing
and offer to install it when you run an ASP.NET Core 2.1 project that uses
HTTPS.

{% img /images/blog/NetCoreSupportInVisualStudioMac7-5/HttpsDevCertificateNotInstalledMessage.png 'HTTPS development certificate not installed message' 'HTTPS development certificate not installed message' %}

Visual Studio for Mac will use the dotnet dev-certs tool to check if the
HTTPS development certificate is installed and trusted.
    
    dotnet dev-certs https --trust --check

Installing the HTTPS certificate requires administrator privileges so you may be
prompted for your username and password after clicking the Yes button. Currently
the prompt for credentials shows **mono-sgen64 wants to make changes**. In the future this will show a
custom message indicating that the credentials are required to install the HTTPS development certificate.

{% img /images/blog/NetCoreSupportInVisualStudioMac7-5/MonoSGenAdminPrompt.png 'Mono sgen administrator dialog prompt' 'Mono sgen administrator dialog prompt' %}

After entering your credentials the following command is run as administrator.
    
    dotnet dev-certs https --trust

After the certificate is installed the ASP.NET Core 2.1 project will be 
opened in the default browser using HTTPS.

{% img /images/blog/NetCoreSupportInVisualStudioMac7-5/AspNetCoreProjectInChromeUsingHttps.png 'ASP.NET Core 2.1 project open in browser using HTTPS' 'ASP.NET Core 2.1 project open in browser using HTTPS' %}

Running **dotnet dev-certs https --trust** to install and trust the certificate needs to be
done with administrator privileges with the user id 0. To do this Visual Studio for Mac
does the following:
    
1. Launches a separate custom console app.
2. The console app uses the AuthorizationExecuteWithPrivileges Mac
  API provided by Xamarin.Mac to launch itself as again as administrator.
  It is not possible to run dotnet as administrator initially since it requires the user id to be
  set to 0, which is what happens when you use **sudo dotnet**, and this can only be done
  when running with administrator privileges. So the console app is launched again but this
  time with administrator privileges.
3. The console app, now being run with administrator privileges, will use setuid to set the
  current user id to 0.
4. The console app then runs **dotnet dev-certs https --trust** which 
 will install and trust the HTTPS development certificate.

The **dotnet dev-certs https --trust** command will add two localhost certificates. You
can see these by opening the Keychain Access application and searching for
localhost.

{% img /images/blog/NetCoreSupportInVisualStudioMac7-5/LocalhostCertificateInKeychain.png 'localhost certificate created by dotnet dev-certs in Keychain Access application' 'localhost certificate created by dotnet dev-certs in Keychain Access application' %}

If the HTTPS development certificate is found to be valid and
trusted then this will be remembered for the current Visual Studio for Mac 
session and a check will not be run again during the current session.

### .NET Core global tools added to path on startup
    
The .NET Core SDK 2.1 supports installing global tools. These tools are .NET Core console apps
that are available as NuGet packages and can be installed and used from the command line.
To be able to use these tools with the dotnet command line the **~/.dotnet/tools**
directory needs to be added to the PATH environment variable. The path to
these global tools is now added to Visual Studio's PATH environment variable 
when it starts.

### Prompt to install .NET Core 2.1 SDK if not installed
    
If a .NET Core 2.1 project is opened and .NET Core 2.1.300 SDK is not
installed then a dialog will be shown allowing the SDK to
be downloaded. The project in the Solution window will show an error
icon indicating that the .NET Core 2.1 SDK is not
installed.

### Launching a browser when running an ASP.NET Core 2.1 project

The .NET Core SDK 2.1 project templates for ASP.NET Core
specify the https and http urls in the launchSettings.json file
by using the applicationUrl property.

		"MyAspNetCore21Project": {
			"commandName": "Project",
			"launchBrowser": true,
			"applicationUrl": "https://localhost:5001;http://localhost:5000",
			"environmentVariables": {
				"ASPNETCORE_ENVIRONMENT": "Development"
			}
		}

With earlier .NET Core 2.1 preview SDKs this was defined in the
ASPNETCORE_URLS environment variable. The full applicationUrl
property was used unmodified when running the ASP.NET Core 2.1 project and 
resulted in an invalid url being used causing the AspNetCoreExecutionHandler to log a warning
and not opening the browser. Now the first url in the applicationUrl
is used if there are multiple urls.

### Support ASPNETCORE_URLS when launching the browser
    
If the launchSettings.json does not define an applicationUrl then
Visual Studio for Mac will fallback to checking the environment variable defined
for ASPNETCORE_URLS in the launchSettings.json file and will use the first url
found there. This url will be used to launch the browser when running the project.
    
An example launchSettings.json that was used in the .NET Core 2.1 preview SDKs
is shown below.

    "MyProject": {
      "commandName": "Project",
      "launchBrowser": true,
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development",
        "ASPNETCORE_URLS": "https://localhost:5001;http://localhost:5000"
      }
    }
    
Compared with the .NET Core SDK 2.0.
    
    "MyProject": {
      "commandName": "Project",
      "launchBrowser": true,
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      },
      "applicationUrl": "http://localhost:5000/"
    }

## Allow .NET Core location to be configured
    
In Preferences there is now a Projects - SDK Locations - .NET Core
section that can be used to configure the location of the .NET Core
command line tool (dotnet).

{% img /images/blog/NetCoreSupportInVisualStudioMac7-5/NetCoreLocationConfiguration.png '.NET Core location configured in preferences' '.NET Core location configured in preferneces' %}

This can be used to configure Visual Studio for Mac to use a
.NET Core SDK that is not installed in the default location. After this is
changed the MSBuild engine hosts are recycled and all .NET Core projects
are re-evaluated to ensure the new locations of any MSBuild targets
are used.

If the location is invalid, or no runtimes or SDKs can be found at the configured
location, a Not found error will be displayed.

{% img /images/blog/NetCoreSupportInVisualStudioMac7-5/NetCoreLocationInvalidPath.png 'Invalid .NET Core location path specified in preferences' 'Invalid .NET Core location path specified in preferences' %}

## Xamarin.Forms Project Templates now include .NET Standard projects
    
The Xamarin.Forms project templates, Blank Forms App and Forms App, will now
create a .NET Standard 2.0 project instead of a Portable Class Library (PCL) project
if .NET Standard is selected in the New Project dialog.

{% img /images/blog/NetCoreSupportInVisualStudioMac7-5/NewProjectDialogFormsNetStandard.png 'New Project dialog - Blank Forms - .NET Standard option' 'New Project dialog - Blank Forms - .NET Standard option' %}

The Xamarin.Forms Class Library project template now creates a .NET Standard 2.0 project instead of
a Portable Class Library project.

{% img /images/blog/NetCoreSupportInVisualStudioMac7-5/NewProjectDialogXamarinFormsLibraryProject.png 'New Project dialog - Xamarin.Forms Class Library project' 'New Project dialog - Xamarin.Forms Class Library project' %}

## Prevent .xaml.cs file from being renamed in a .NET Core project
    
The Xamarin.Forms NuGet package has an MSBuild .targets file that is imported
after the project items are defined. This .targets file overrides
the DependentUpon property for all .xaml.cs files. This means that renaming the
.xaml.cs file to be different to the .xaml file is not supported.
To prevent this the Rename menu is now disabled in the Solution
window for .xaml.cs files if they depend on a .xaml file.

## Exclude None build action for XAML files in .NET Core projects
    
Recent versions of Xamarin.Forms NuGet packages exclude all .xaml files from the
default None file wildcard defined by the .NET Core SDK. This exclusion is
done in a .targets file which is applied after the items in the project
file have been added. This means that a .NET Standard project should not use
None items for .xaml files since they will be removed. To avoid this problem
the None build action is excluded from the list of build actions for .xaml files. 
This list of build actions is available when right
clicking the file in the Solution window and in the Properties window when
the file is selected.

## Dependent files now renamed on renaming parent file in the Solution window
    
When a file is renamed in the Solution window the dependent files will
also be renamed if they start with the same name as the parent file.
This avoids problems with XAML files since a different name for the .xaml file
and the associated .xaml.cs file is not supported.

## Improve project load times for projects with many files

Opening a .NET Core project that contained many files that 
were not excluded, such as a .NET Core console project that has a
node_modules directory, could take a long time to load.

Some performance improvements have been made to speed up the
loading of .NET Core projects. For a .NET Core console project that had a node_modules 
directory containing around 17000 files the initial project load time which
was taking around 70-80 seconds and now it takes around 20 seconds. Visual Studio 2017 on
Windows takes around 15-20 seconds to load the same project before
it is visible in the Solution Explorer window.

The performance improvements include:

1. Adding a faster project item lookup used when finding an existing
project item on loading the project.
2. Updating the existing project items is now faster avoiding iterating
over the existing items.
3. Evaluating MSBuild items is now only done when evaluating properties.
Previously this was done when evaluating project configurations and run configurations.
This would result in
files and directories being searched multiple times when looking
wildcard matches on loading the project. Now the files
and directories are searched once during the initial project load.

## .NET Core templating engine updated
    
Updated the .NET Core templating to version 1.0.0-beta3-20171117-314. This new 
version of the .NET Core templating engine fixes the following problems:
    
1. Templates that use files with @ character in their names being generated with the
@ symbol encoded.
2. Templates that use the Guid macro and did not specify a format
would cause the template generation to fail. An exception was thrown
since the format was not defined. Now 
if the format is not defined the default format is used.

## Report template creation failures when using the .NET Core templating engine

To help diagnose problems with project and item templates that use the
.NET Core templating engine more detailed information about the failure is now logged
in the IDE log.

## Support item templates with the new templating engine
    
Item templates that use the .NET Core
templating engine can be defined through a new extension point.
    
    <Extension path="/MonoDevelop/Ide/ItemTemplates">
            <Template
                    id="Azure.Function.CSharp.BlobTrigger"
                    _overrideName="Blob Trigger"
                    path="Templates/Azure.Functions.Templates.nupkg" />
    </Extension>
    
Item templates are not currently supported in the New File dialog however there
is an API that can be used by extensions to create files from these templates.
This is currently used when adding a new Azure Function to a project.

## Bug Fixes

**Missing child package dependencies in Solution window**

After creating a new ASP.NET Core project sometimes the package dependencies
shown in the Solution window under the SDK folder and the NuGet folder 
could not be expanded to view the child dependencies.

The problem was that if the call to get the package dependencies
using MSBuild was cancelled this would result an empty list of depenencies
being cached. Since no package dependencies were returned the Solution window
would fallback to showing the package dependencies that were listed in the
project and a default top level SDK package dependency.

**MSBuild wildcards being expanded on saving a project**
    
Saving a project with a file wildcard that had a Link with
MSBuild item metadata, as shown below, would result in the wildcard
being removed and replaced with MSBuild items for each file included
by the wildcard.
    
    <Compile Include="**\*.cs" Exclude="obj\**">
      <Link>%(RecursiveDir)%(Filename).cs</Link>
    </Compile>
    
The problem was that the evaluated Link property value was being
compared with the unevaluated value, which did not match, resulting
in the wildcard being replaced with individual MSBuild items for
each file. Now if the property value contains a % character a comparison is
made against the evaluated value when checking if the project item
has changed.

**MSBuild items added for wildcards with link metadata on saving project**
    
A project containing the following MSBuild file wildcards would have
extra MSBuild items added when the project file was saved.
    
    <ItemGroup>
      <Compile Include="..\**\*.cs">
        <Link>%(RecursiveDir)%(Filename)%(Extension)</Link>
      </Compile>
    </ItemGroup>
    
On saving the project Compile update items would be added for each
file included by the file wildcard.
    
    <ItemGroup>
      <Compile Update="..\Test\Class1.cs">
        <Link>Class1.cs</Link>
      </Compile>
    </ItemGroup>

This problem is similar to the previous problem where the evaluated Link value was being
compared with the unevaluated value, which did not match, resulting
MSBuild update items being added to the project when it was saved. Now if the property 
value contains a % character a comparison is made against the evaluated value when checking if 
the project item has changed.

**TargetFramework changed on saving project**
    
Adding a file to an SDK style project that targeted Tizen 4.0 would
result in the TargetFramework changing from **tizen40** to **tizen4.0**.
Now the original framework identifier name is not modified and if
the version of the framework changes then the version will be
dotted or contain only numbers based on the format that was originally used
in the project file.

**ASP.NET Core file templates modifying project file**
    
Adding a new .cshtml file from a file template to an ASP.NET Core
project would modify the project file when it should not have been
modified. The problem was the files were added as None items whilst
.cshtml are Content items. The project file would contain the
following after adding a new cshtml file from a template:
    
    <ItemGroup>
      <Content Remove="Views\Index.cshtml" />
    </ItemGroup>
    <ItemGroup>
      <None Include="Views\Index.cshtml" />
    </ItemGroup>
    
Another problem was that the Razor Page with view model file
template specifies a DependentUpon property so this was added to the
project file. This would result in the .cshtml and .cs files being nested whilst
the other .cshtml and .cs files created with the initial ASP.NET
Core project template were not nested. The project file would include
the following:
    
    <ItemGroup>
      <Compile Update="Views\Index.cshtml.cs">
        <DependentUpon>Index.cshtml</DependentUpon>
      </Compile>
    </ItemGroup>
    
The .NET Core SDK does not indicate that .cshtml and .cs
files are dependent on each other so they are not currently nested
in the Solution window. New .cshtml files created from these updated
file templates will now not be nested in Solution window.

**XAML files not nested after editing project file in editor**
    
Adding a new content page with xaml to a .NET Standard project,
then excluding the files from the project, but not deleting it,
then editing the project file and commenting out the MSBuild remove
items, would then result in the xaml files not being nested in the
Solution window. The problem was that the MSBuild update item
for the .xaml.cs file, defined by the Xamarin.Forms default msbuild
items, was being removed from the MSBuild project in
memory. This MSBuild update item had the DependsOn property defined
so this information was lost on reloading the project. Now a check
is to made to ensure only update items that exist in the original
project file are removed.

**MSBuild item added for XAML file copied to a .NET Standard project**
    
When copying a .xaml file from a Portable Class Library project to a .NET Standard project
an MSBuild include item for the file would be added if the .xaml file did
not have the default metadata properties defined by Xamarin.Forms.
Now .NET Core projects opt-in to supporting items not being excluded if they
are missing MSBuild item metadata
which prevents an MSBuild include item added for the .xaml file.

**Duplicate XAML file build error when copying file to a .NET Standard project**

Copying a .xaml file from a Portable Class Library project to a .NET Standard project 
would result an MSBuild item for the .xaml file to the project causing it to not
build due to a duplicate .xaml file.

Now when a xaml file is copied into a .NET Standard project, and it is missing 
properties that are included in by an update wildcard,
an MSBuild item will not be added to the project.

**MSBuild remove item not added for .xaml.cs file**
    
With a .NET Core project, containing a single .xaml file and a
dependent .xaml.cs file, removing but not deleting the .xaml file
from the Solution window would not add an MSBuild remove item for
the .xaml.cs file even though it was removed from the Solution window.
    
The problem was that the Xamarin.Forms NuGet package includes a
Compile update item and only this was being considered when saving
the project file. The default file wildcard, that includes all .cs files,
provided by the .NET Core SDK was not considered. Only the last MSBuild item 
associated with the .xaml.cs file was being considered. If there was another .cs 
file in the project then the MSBuild remove item was added correctly. To avoid this 
all MSBuild items associated with a project item are now considered.

**MSBuild remove item not removed on adding XAML file to project**
    
When an .xaml file was removed but not deleted, and Xamarin.Forms 2.5
used, which has default MSBuild items defined, on adding the .xaml
file back again to the project the EmbeddedResource
remove item was not removed from the project.
    
The problem was that the .xaml file was being added as a None item
since by default there is no build action specified for .xaml files.
    
Another problem was that the MSBuild remove
items, defined by Xamarin.Forms that remove all .xaml files from the default None 
included by the .NET Core SDK, were being ignored since the file wildcards were not
found.
    
Also an msbuild item is no longer added for an existing xaml file when it is
added to the project. The Xamarin.Forms default msbuild items
for .xaml files have extra metadata which were not being added to
the file when it was added to the project from the file system.

**Update item added on renaming xaml.cs file**

When the .xaml and .xaml.cs file were renamed at the same time an
MSBuild update item was added for the .xaml.cs file even though the Xamarin
Forms NuGet package has a default MSBuild item that was the same as
the generated MSBuild update item.

**Argument null exception logged on opening .NET Core project**
    
Opening a .NET Core project would sometimes log an unhandled
ArgumentNullException. A file created event was sometimes raised before
the project had finished loading and could result in
a null set of items being used when checking if the
new file should be added to the project. This is now handled. The files being
created on project load are typically in the obj folder and would be
ignored by the default file wildcards.
    
    An unhandled exception has occured. Terminating Visual Studio? False
    System.ArgumentNullException: Value cannot be null.
    Parameter name: source
      at System.Linq.Enumerable.Where[TSource]
      in corefx/src/System.Linq/src/System/Linq/Where.cs:42
      at MonoDevelop.Projects.Project.OnFileCreatedExternally
    in src/core/MonoDevelop.Core/MonoDevelop.Projects/Project.cs:4041
  