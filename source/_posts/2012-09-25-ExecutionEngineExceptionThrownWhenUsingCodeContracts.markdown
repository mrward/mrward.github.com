---
layout: post
title: "ExecutionEngineException thrown when using Code Contracts"
date: 2012-09-25 17:35
comments: true
categories: CodeContracts
---

Whilst changing the Entity Framework 5.0 NuGet package to work with [SharpDevelop](http://www.icsharpcode.net/OpenSource/SD/) a custom build of the NuGet package was causing Visual Studio 2010 to crash when the package was being installed. Debugging the crash by attaching another instance of Visual Studio revealed that an [ExecutionEngineException](http://msdn.microsoft.com/en-us/library/system.executionengineexception.aspx) was being thrown when the following line of code was being executed.

    Contract.Requires(project != null);

The exception itself had no other information. The exception's message just repeated the exception that was being thrown: "Exception of type 'System.ExecutionEngineException' was thrown.". This exception indicates an internal error occurred in the CLR execution engine.

The real problem is the use of [Code Contracts](http://research.microsoft.com/en-us/projects/contracts/) and not following the [Entity Framework documentation on how to build it from source code](http://entityframework.codeplex.com/documentation).

The [Contract](http://msdn.microsoft.com/en-us/library/system.diagnostics.contracts.contract.aspx) class is used to define required preconditions and postconditions for a method and is used when defining Code Contracts. It is part of the .NET Framework 4.0 and can be found in mscorlib.dll in the System.Diagnostics.Contracts namespace.

The problem with the custom build of Entity Framework is that there are parts of Code Contracts included with .NET Framework 4.0 so you can compile a project that uses Code Contracts without any errors but on running the application you can have an ExecutionEngineException thrown. This can occur if you do not have the [Code Contracts Visual Studio extension](http://msdn.microsoft.com/en-us/devlabs/dd491992.aspx) installed when the project is built. With this extension installed your assembly will be modified when it is compiled by the code contracts binary rewriter.  The rewriter will inject the contracts into your assembly so the checks will be run when the code is executed.

##Solution

To get around this problem you have two options.

1. Install the [Code Contracts](http://msdn.microsoft.com/en-us/devlabs/dd491992.aspx) Visual Studio extension and then rebuild your project. After installing you should have a Code Contracts page in your project options.

2. Remove [CONTRACTS_FULL](http://msmvps.com/blogs/luisabreu/archive/2008/11/12/code-contracts-and-runtime-rewriting.aspx) from the conditional compilation symbols in the project's options.

CONTRACTS_FULL is used to enable runtime checking of the code contracts. If this is not defined then the calls made to the any of Contract methods are ignored by the compiler when building your project.
