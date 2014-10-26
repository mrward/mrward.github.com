---
layout: post
title: "Hosting Google's V8 JavaScript Engine on Mono"
date: 2014-01-26 12:27
comments: true
categories: mono V8
---

How can we host [Google's V8 JavaScript engine](https://code.google.com/p/v8/) inside a .NET application that is running on Mono? The ultimate goal is to allow the TypeScript addin written for SharpDevelop to be ported so it can run on Linux and Mac inside Xamarin Studio and MonoDevelop.

The [TypeScript addin for SharpDevelop](http://community.sharpdevelop.net/blogs/mattward/archive/2013/04/24/TypeScriptSupportInSharpDevelop.aspx) uses the [JavaScript.NET](http://javascriptdotnet.codeplex.com/) library to host Google's V8 JavaScript engine, execute JavaScript code and allow the JavaScript code to interfact with .NET objects. JavaScript.NET is written in Managed C++ which is unsupported on Mono so we need to look elsewhere. 

 What are the options for hosting V8 on .NET?

1. [JavaScript.NET](http://javascriptdotnet.codeplex.com/) - Managed C++
2. [ClearScript.NET](http://clearscript.codeplex.com/) - Managed C++ and C#
3. [V8 Sharp](http://v8sharp.codeplex.com/) - Managed C++.
4. [V8.NET](http://v8dotnet.codeplex.com/) - C++ and C#.
5. [Node]().

V8 itself is written in C++. For Mono we will need a compiled version of V8 for the operating system we are interested in. We will also need an API that allows V8 to be used from Mono. This will need to be a C API so we can call it directly from C#. All the libraries listed above all use Managed C++ to create a .NET assembly that can be used from within a .NET application to host V8. For Mono we will need either a dll that exposes a C API and a .NET assembly written in C# that calls that API.

V8.NET looks like a good choice since it is written in C++ and exposes a C API that can be consumed from C#.

Using Node to host V8 and then interacting from .NET by sending commands to Node  is another interesting idea. This approach is being worked on by Atsushi Eno and the source code for a [MonoDevelop addin is available on GitHub](https://github.com/atsushieno/md-typescript).

 



