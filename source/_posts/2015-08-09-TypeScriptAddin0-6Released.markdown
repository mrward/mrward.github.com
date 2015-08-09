---
layout: post
title: "TypeScript Addin 0.6 Released"
date: 2015-08-09 15:00
comments: true
categories: TypeScript Xamarin MonoDevelop
---

A new version of the TypeScript addin for Xamarin Studio and MonoDevelop has been released. The addin is available from [MonoDevelop's Add-in Repository](http://addins.monodevelop.com/) in the alpha channel. More details on how to install the addin can be found in the [TypeScript support in Xamarin Studio post](/blog/2015/04/01/TypeScriptSupportInXamarinStudio/).

## Changes

* Updated to support [TypeScript 1.5](http://blogs.msdn.com/b/typescript/archive/2015/07/20/announcing-typescript-1-5.aspx).
* Linux 32 bit and 64 bit are now supported with a single addin. Thanks to [Christian Bernasko](https://github.com/chrisber).
* Allow UMD and System modules to be selected in project options.

The separate TypeScript Linux 32 bit addin is now deprecated since the TypeScript addin can now be used on 32 bit and 64 bit versions of Linux.

## Bug Fixes

* TypeScript language service host not updated when the project options are changed

For example, switching from ES3 to ES6 in the project options could cause the code completion to be incorrect since the language service host compiler settings were not being updated.
 
* TypeScript options shown when viewing solution options

The TypeScript options are now only available when the project options are selected.