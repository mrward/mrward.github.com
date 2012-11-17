---
layout: post
title: "IronPython Error: Attribute of Namespace Object is Read-Only"
date: 2012-11-17 14:40
comments: true
categories: IronPython
---

If you are seeing an AttributeError, as shown below, when running IronPython 2.7 then check you have added all the required assembly reference to your application.

    AttributeError: attribute 'Xml' of 'namespace#' object is read-only

The attribute name displayed will most likely be different but the underlying problem is that you are trying to use a type that exists in an assembly that is not referenced. Here is a simple example that reproduces the error.

    import System
    doc = System.Xml.XmlDocument()

The code above is importing the System namespace and then attempting to create an XmlDocument object. Running the above code with ipy.exe will result in the AttributeError being displayed.

	D:\projects>ipy
	IronPython 2.7.3 (2.7.0.40) on .NET 4.0.30319.296 (32-bit)
	Type "help", "copyright", "credits" or "license" for more information.
	>>> import System
	>>> doc = System.Xml.Document()
	Traceback (most recent call last):
	  File "<stdin>", line 1, in <module>
	AttributeError: attribute 'Xml' of 'namespace#' object is read-only
	>>>

The cryptic error message indicates that we do not have a reference to the System.Xml assembly. To fix the problem we add the following two lines of code before the code that creates the XmlDocument object.

    import clr
    clr.AddReference("System.Xml")
    
Running the code again will now cause the XmlDocument object to be created without an error.
    
	D:\projects>ipy
	IronPython 2.7.3 (2.7.0.40) on .NET 4.0.30319.296 (32-bit)
	Type "help", "copyright", "credits" or "license" for more information.
	>>> import clr
	>>> clr.AddReference("System.Xml")
	>>> import System
	>>> doc = System.Xml.XmlDocument()
	>>> doc.ToString()
	'System.Xml.XmlDocument'
	>>>
