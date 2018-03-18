---
layout: post
title: "Language Server Protocol support in Visual Studio for Mac 7.4"
date: 2018-03-18 12:30
comments: true
categories: Xamarin MonoDevelop VSMac LanguageServerProtocol
---

A preview of support for the [Language Server Protocol](https://github.com/Microsoft/language-server-protocol) 
is now available for Visual Studio for Mac 7.4 as a separate extension.

{% img /images/blog/LanguageServerSupportInVisualStudioMac/DockerLanguageServerClient.gif 'Docker Language Server client being used in Visual Studio for Mac' 'Docker Language Server client being used in Visual Studio for Mac' %}

A Language Server can provide support for programming language features such as:

 - Code completion
 - Find references
 - Go to definition
 - Quick fixes
 - Method signature help
 - Errors and warnings diagnostics
 - Documentation on hover
 - Document formatting
 - Rename refactoring

The [Language Server Protocol](https://github.com/Microsoft/language-server-protocol) provides a way for a
client application, such as Visual Studio for Mac, to communicate with the Language Server in order to
use the programming language features it supports.

[Visual Studio Code](https://code.visualstudio.com) supports the 
[Language Server Protocol](https://code.visualstudio.com/blogs/2016/06/27/common-language-protocol). 

A preview of [Language Server Protocol support in Visual Studio 2017 on Windows](https://blogs.msdn.microsoft.com/visualstudio/2017/11/21/announcing-language-server-protocol-preview-release/)
has also been announced. 

Currently there is no support for debugging. Whilst there is a 
[debugging protocol](https://code.visualstudio.com/docs/extensionAPI/api-debugging) support
for this is not currently available in Visual Studio for Mac.

The Language Server Protocol does not provide any support for compiling the code. This would need
to be provided by the extension that implements the language client.

More detailed information about the Language Server Protocol can be found on the 
[Language Server Protocol site](https://microsoft.github.io/language-server-protocol/).

The **Creating a Language Client** section below will take a more detailed look at how to implement
a custom Language Server Client extension in Visual Studio for Mac.

## Supports

 - MonoDevelop or Visual Studio Mac 7.4 or later.

## Source Code

- [Language Server Client extension for Visual Studio for Mac and MonoDevelop](https://github.com/mrward/monodevelop-language-server-addin)

## Installation

The Language Server Client extension is available to download from [GitHub](https://github.com/mrward/monodevelop-language-server-addin/releases/download/0.1-monodevelop-7.4/MonoDevelop.LanguageServer.Client_0.1.mpack).

To install the extension open the Extensions Manager by selecting Extensions... from the main menu. Click the Install from file button. Select the .mpack file and then click the Open button.

The extension is also available from a [custom extension server](https://github.com/mrward/monodevelop-addins). 
It is not currently published to the main Visual Studio for Mac extension gallery.

## Creating a Language Client

The API provided by the Language Server Client extension follows the [API defined by the
Language Server Protocol extension used by Visual Studio on Windows](https://docs.microsoft.com/en-us/visualstudio/extensibility/adding-an-lsp-extension) as
closely as possible.

More detailed documentation on the [Language Server Client API](https://docs.microsoft.com/en-us/visualstudio/extensibility/language-server-protocol) provided 
by Visual Studio on Windows is available from the [Microsoft docs site](https://docs.microsoft.com/en-us/visualstudio/extensibility/language-server-protocol).

### Prerequisites

The [Language Server Client Extension](https://github.com/mrward/monodevelop-language-server-addin) for 
Visual Studio for Mac should be installed.

Also ensure you have the [Addin Maker extension installed](https://github.com/mhutch/MonoDevelop.AddinMaker).
This can be installed by selecting Extensions... from the main menu top open the Extensions Manager.
Select the Gallery tab and use the text box at the top right to search for the Addin Maker extension.

{% img /images/blog/LanguageServerSupportInVisualStudioMac/ExtensionsDialogAddinMaker.png 'Addin maker selected in Extensions Manager dialog' 'Addin maker selected in Extensions Manager dialog' %}

Click the Install button to install the Addin Maker extension.

The Addin Maker will be used to create the language client extension.

### Creating an IDE Extension project

The starting point is to create an IDE Extension project. This project template is provided by the
Addin Maker extension and is available 
from the New Project dialog, in the Other - IDE Extensions category.

{% img /images/blog/LanguageServerSupportInVisualStudioMac/NewProjectDialogIdeExtension.png 'IDE Extension project selected in New Project dialog' 'IDE Extension project selected in New Project dialog' %}

After creating the IDE Extensions project, expand the Dependencies folder to show the
Extensions folder. Right click the Extensions folder and select Add Addin Reference.

{% img /images/blog/LanguageServerSupportInVisualStudioMac/NewProjectDialogIdeExtension.png 'IDE Extension project selected in New Project dialog' 'IDE Extension project selected in New Project dialog' %}

Find the MonoDevelop.LanguageServer.Client in the Add Extension Reference dialog. Select it to
toggle its check box and then click the Add button to reference it.

{% img /images/blog/LanguageServerSupportInVisualStudioMac/AddExtensionReferenceDialogLanguageServerClient.png 'Language Server Client extension in Add Extension Reference dialog' 'Language Server Client extension in Add Extension Reference dialog' %}

There are three parts to implementing a language client:

 - Enabling [Managed Extensibility Framework (MEF)](https://github.com/Microsoft/vs-mef) support
 - Defining the supported content
types
 - Implementing the ILanguageClient interface.
 
First we will take a look at enabling MEF support.

### Enabling Managed Extensibility Framework Support

Registration of the language client with Visual Studio for Mac is done using the [Managed Extensibility Framework (MEF)](https://github.com/Microsoft/vs-mef) 
which is supported by Visual Studio for Mac.

In order to use MEF in your language client extension you need to indicate to Visual Studio
for Mac that it should scan your extension for dependencies. This can be done in the
extension's .addin.xml file by adding the filename of your assembly.

```
	<Extension path="/MonoDevelop/Ide/Composition">
		<Assembly file="MockLanguageExtension.dll" />
	</Extension>
```

### Content Type Definition

Your language client needs to indicate what files are supported. This is done by defining
a content type definition.

```
using System.ComponentModel.Composition;
using Microsoft.VisualStudio.LanguageServer.Client;
using Microsoft.VisualStudio.Utilities;

namespace MockLanguageExtension
{
	#pragma warning disable CS0649 // Field is never assigned to.

	public class FooContentDefinition
	{
		[Export]
		[Name("foo")]
		[BaseDefinition(CodeRemoteContentDefinition.CodeRemoteContentTypeName)]
		internal static ContentTypeDefinition FooContentTypeDefinition;

		[Export]
		[FileExtension(".foo")]
		[ContentType("foo")]
		internal static FileExtensionToContentTypeDefinition FooFileExtensionDefinition;
	}

	#pragma warning restore CS0649
}
```

The above content definition indicates that the .foo file extension is supported by the language client.

You can also define a filename, instead of a file extension, as supported by the language client by using
the FileName attribute instead of the FileExtension attribute.

```
	class DockerContentTypeDefinition
	{
		[Export]
		[Name("docker")]
		[BaseDefinition(CodeRemoteContentDefinition.CodeRemoteContentTypeName)]
		internal static ContentTypeDefinition contentTypeDefinition;

		[Export]
		[FileName("Dockerfile")]
		[ContentType("docker")]
		internal static FileExtensionToContentTypeDefinition dockerFileDefinition;
	}
```

You will need to add a reference to **System.ComponentModel.Composition** so the [Export attribute](https://msdn.microsoft.com/library/system.componentmodel.composition.exportattribute.aspx)
can be used.

### Implementing ILanguageClient

To integrate the Language Server with Visual Studio for Mac the 
[ILanguageClient interface](https://docs.microsoft.com/en-us/dotnet/api/microsoft.visualstudio.languageserver.client.ilanguageclient)
needs to be implemented.

```
using System;
using System.Collections.Generic;
using System.ComponentModel.Composition;
using System.Diagnostics;
using System.IO;
using System.Threading;
using System.Threading.Tasks;
using Microsoft.VisualStudio.LanguageServer.Client;
using Microsoft.VisualStudio.Threading;
using Microsoft.VisualStudio.Utilities;

namespace MockLanguageExtension
{
	[ContentType("foo")]
	[Export(typeof(ILanguageClient))]
	public class FooLanguageClient : ILanguageClient
	{
		public IEnumerable<string> ConfigurationSections => null;

		public object InitializationOptions => null;

		public IEnumerable<string> FilesToWatch => null;

		public string Name => "Foo Language Extension";

		public event AsyncEventHandler<EventArgs> StartAsync;
		public event AsyncEventHandler<EventArgs> StopAsync;

		public async Task OnLoadedAsync()
		{
			await StartAsync?.InvokeAsync(this, EventArgs.Empty);
		}

		public Task<Connection> ActivateAsync(CancellationToken token)
		{
			var info = new ProcessStartInfo();
			var programPath = Path.Combine(Path.GetDirectoryName(GetType().Assembly.Location), "server", @"LanguageServer.UI.exe");
			info.FileName = "mono";
			info.Arguments = programPath;
			info.WorkingDirectory = Path.GetDirectoryName(programPath);
			info.UseShellExecute = false;
			info.RedirectStandardInput = true;
			info.RedirectStandardOutput = true;

			var process = new Process();
			process.StartInfo = info;

			Connection connection = null;

			if (process.Start())
			{
				connection = new Connection(process.StandardOutput.BaseStream, process.StandardInput.BaseStream);
			}

			return Task.FromResult(connection);
		}
	}
}
```

The class that implements the ILanguageClient interface needs to indicate it supports
the content types that you have defined, and it also needs to indicate that it implements the
ILanguageClient by using the Export attribute.

```
	[ContentType("foo")]
	[Export(typeof(ILanguageClient))]
```
The language client should return a unique description for the ILanguageClient.Name
property.

The two methods that should be implemented are:

 - [OnLoadedAsync](https://docs.microsoft.com/en-us/dotnet/api/microsoft.visualstudio.languageserver.client.ilanguageclient.onloadedasync) 
 - [ActivateAsync](https://docs.microsoft.com/en-us/dotnet/api/microsoft.visualstudio.languageserver.client.ilanguageclient.activateasync).

OnLoadedAsync is called when Visual Studio for Mac has loaded your extension. To activate
your Language Server you need to call StartAsync. The simplest approach is to call StartAsync
in the OnLoadedAsync method.

```
		public async Task OnLoadedAsync()
		{
			await StartAsync?.InvokeAsync(this, EventArgs.Empty);
		}
```

Calling StartAsync will cause Visual Studio for Mac to call the ILanguageClient's
ActivateAsync method. This is where you can start the Language Server.

The ActivateAsync method should return a Connection object with the streams that
will be used to communicate with the Language Server. Standard input and output streams
are supported as well as sockets.

```
var connection = new Connection(process.StandardOutput.BaseStream, process.StandardInput.BaseStream);
```

The first time you try to run and debug your language client extension you will see an error
in the Application Output similar to:

```
WARNING: The add-in 'MockLanguageExtension,1.0' could not be updated because some of its 
dependencies are missing or not compatible:
  missing: MonoDevelop.LanguageServer.Client,0.1
```

The Addin Maker runs your language client extension using its own extension database. The
MonoDevelop.LanguageServer.Client extension needs to be installed into this separate database
in order to be able to run and debug your language client extension.

To do this run your language client extension project in Visual Studio for Mac and 
install the Language Server Client extension using the Extensions Manager in the instance
of Visual Studio for Mac that is started. Then stop debugging and re-run your extension project 
again, and your language client should now be used when you open a supported file.

The content type registration and the ILanguageClient implementation is the minimum
that is needed in order to integrate your Language Server with Visual Studio for Mac.
Now let us take a look at other optional things that can be implemented.

### Receiving Custom Messages

Your Language Server may support messages that are not part of the standard
Language Server Protocol.

In order to support receiving custom messages the class that implements ILanguageClient should also 
implement the 
[ILanguageClientCustomMessage](https://docs.microsoft.com/en-us/dotnet/api/microsoft.visualstudio.languageserver.client.ilanguageclientcustommessage)
interface. The CustomMessageTarget property should return an object that will receive the messages.

```
public object CustomMessageTarget => new CustomMessageTarget ();
public object MiddleLayer => null;

public Task AttachForCustomMessageAsync(JsonRpc rpc)
{
	return Task.CompletedTask;
}
```

An example CustomMessageTarget class is shown below.

```
using Newtonsoft.Json.Linq;
using StreamJsonRpc;

namespace MockLanguageExtension
{
	public class CustomMessageTarget
	{
		[JsonRpcMethod("test/customNotification")]
		public void OnCustomNotification(JToken arg)
		{
			// Handle custom message from the Language Server.
		}
	}
}
```

### Sending Custom Messages

To send custom messages to the Language Server the class that implements the
ILanguageClient interface should also implement the 
[ILanguageClientCustomMessage](https://docs.microsoft.com/en-us/dotnet/api/microsoft.visualstudio.languageserver.client.ilanguageclientcustommessage) interface.

The AttachForCustomMessageAsync method
should save the JsonRpc object passed and then you can use the JsonRpc object to
send custom messages to the Language Server.

```
		JsonRpc rpc;

		public Task AttachForCustomMessageAsync(JsonRpc rpc)
		{
			this.rpc = rpc;
			return Task.CompletedTask;
		}

		public async Task SendMessageToServer()
		{
			string text = await rpc.InvokeWithParameterObjectAsync<string>("GetText");
			MessageService.ShowMessage("Text from language server:", text);
		}
```

### Middle Layer

The ILanguageClientCustomMessage defines a MiddleLayer property. An object returned
from the MiddleLayer property can be used to intercept messages sent to and received
from the Language Server. The MiddleLayer object can implement the following
interfaces:

 - [ILanguageClientCompletionProvider](https://docs.microsoft.com/en-us/dotnet/api/microsoft.visualstudio.languageserver.client.ilanguageclientcompletionprovider)
 - [ILanguageClientExecuteCommandProvider](https://docs.microsoft.com/en-us/dotnet/api/microsoft.visualstudio.languageserver.client.ilanguageclientexecutecommandprovider)
 - [ILanguageClientWorkspaceSymbolProvider](https://docs.microsoft.com/en-us/dotnet/api/microsoft.visualstudio.languageserver.client.ilanguageclientworkspacesymbolprovider)

```
using System;
using System.Threading.Tasks;
using Microsoft.VisualStudio.LanguageServer.Client;
using Microsoft.VisualStudio.LanguageServer.Protocol;

namespace MockLanguageExtension
{
	class MiddleLayerProvider : ILanguageClientCompletionProvider
	{
		public Task<object> RequestCompletions(
			TextDocumentPositionParams param,
			Func<TextDocumentPositionParams, Task<object>> sendRequest)
		{
			return sendRequest(param);
		}

		public async Task<CompletionItem> ResolveCompletion(
			CompletionItem item,
			Func<CompletionItem, Task<CompletionItem>> sendRequest)
		{
			return await sendRequest(item);
		}
	}
}
```

Note that currently not all the Language Server messages can be intercepted.

The full source code to the sample Language Server Client Extension created
whilst writing this blog post is available on 
[GitHub](https://github.com/mrward/sample-language-server-client-extension)

There are other examples included with the 
[Language Server Client extension source code](https://github.com/mrward/monodevelop-language-server-addin)
on GitHub, however these are not standalone and are compiled with the
Language Server Client extension.