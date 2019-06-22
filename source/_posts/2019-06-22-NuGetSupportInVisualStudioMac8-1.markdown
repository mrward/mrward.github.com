---
layout: post
title: "NuGet Support in Visual Studio for Mac 8.1"
date: 2019-06-22 12:40
comments: true
categories: NuGet Xamarin VSMac MonoDevelop
---

## Changes

   * NuGet 5.0 support
   * Fixed PackageReference metadata not added for development dependencies
   * Fixed DotNetCliToolReferences not being restored
   * Fixed Multiplatform Library build error with Android projects

More information on all the new features and changes in [Visual Studio for Mac 8.1](https://www.visualstudio.com/vs/visual-studio-mac/)
can be found in the [release notes](https://docs.microsoft.com/en-us/visualstudio/releasenotes/vs2019-mac-relnotes).

## NuGet 5.0 support

[NuGet 5.0.2.5988](https://docs.microsoft.com/en-us/nuget/release-notes/nuget-5.0-rtm) is now
included with Visual Studio for Mac 8.1.

## Bug Fixes

**Fixed PackageReference metadata not added for development dependencies**

Installing a NuGet package that is a development dependency, such as
GitInfo, would not add the PrivateAssets nor the IncludeAssets
metadata to the PackageReference. This is now supported and mirrors the
behaviour of 'dotnet add package' and Visual Studio on Windows.

**Fixed DotNetCliToolReferences not being restored**

DotNetCliToolReferences are available in the package dependency graph
but are treated as separate projects in this graph. Since these did not map
to an existing project in the solution they were not added to the full dependency
graph which resulted in these tools not being restored.

DotNetCliToolReferences are only restored when the entire solution is restored.
Restoring a single project only restores the project itself not
the dotnet cli tool project referenced by the project.

MSBuild supports restoring DotNetCliToolReferences in any project type that
uses PackageReferences so Visual Studio for Mac also supports this.

**Fixed Multiplatform Library build error with Android projects**

When generating a Portable Class Library (PCL) assembly from the intersection of project
assemblies the ApiIntersect build task would throw an exception since
it could not resolve the Mono.Android assembly. This problem has been
fixed in a more recent NuGet.Build.Packaging where the failure to
resolve has been converted to a warning.

```
System.Exception: Could not resolve Mono.Android, Version=0.0.0.0, Culture=neutral, PublicKeyToken=84e04ff9cfb79065
    at ApiIntersect.FrameworkAssemblyResolver.Resolve (Mono.Cecil.AssemblyNameReference name, Mono.Cecil.ReaderParameters parameters) [0x0001b] in <aac3e0d5bcd4473a96e385115da49b96>:0
    at ApiIntersect.FrameworkAssemblyResolver.Resolve (Mono.Cecil.AssemblyNameReference name) [0x00000] in <aac3e0d5bcd4473a96e385115da49b96>:0
    at ICSharpCode.Decompiler.Ast.Transforms.IntroduceUsingDeclarations.Run (ICSharpCode.NRefactory.CSharp.AstNode compilationUnit) [0x00142] in <37b5ad8a7a94479fbc5b574a8fc6281a>:0
    at ICSharpCode.Decompiler.Ast.Transforms.TransformationPipeline.RunTransformationsUntil (ICSharpCode.NRefactory.CSharp.AstNode node, System.Predicate`1[T] abortCondition, ICSharpCode.Decompiler.DecompilerContext context) [0x0002c] in <37b5ad8a7a94479fbc5b574a8fc6281a>:0
    at ICSharpCode.Decompiler.Ast.AstBuilder.RunTransformations (System.Predicate`1[T] transformAbortCondition) [0x00000] in <37b5ad8a7a94479fbc5b574a8fc6281a>:0
    at ICSharpCode.Decompiler.Ast.AstBuilder.RunTransformations () [0x00000] in <37b5ad8a7a94479fbc5b574a8fc6281a>:0
    at ApiIntersect.MainClass.DumpTypes (System.Collections.Generic.List`1[T] types, System.String baseDir) [0x000a7] in <aac3e0d5bcd4473a96e385115da49b96>:0
    at ApiIntersect.MainClass.Process (System.Collections.Generic.List`1[T] intersections, System.Collections.Generic.List`1[T] exclusions, Mono.Cecil.ReaderParameters readerParameters, System.String outputPath) [0x00319] in <aac3e0d5bcd4473a96e385115da49b96>:0
    at ApiIntersect.MainClass.Main (System.String[] args) [0x0039f] in <aac3e0d5bcd4473a96e385115da49b96>:0
```

An updated NuGet.Build.Packaging has not been published to nuget.org
so only new projects created with Visual Studio for Mac will get the
NuGet package containing the fix.
