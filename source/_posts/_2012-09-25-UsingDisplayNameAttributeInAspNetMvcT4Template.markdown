---
layout: post
title: "Using DisplayName Attribute in ASP.NET MVC T4 Template"
date: 2012-09-25 11:34
comments: true
categories: ASP.NET MVC T4
---
Using DisplayName Attribute in MVC T4 Template


How do you use the DisplayName for a class in an MVC T4 Template?

Given the following class, how can the DisplayName attribute be 


[MVC Scaffolding](http://mvcscaffolding.codeplex.com/) templates use the Visual Studio object model which is different to how the standard ASP.NET MVC templates work. The Model.ViewDataType is a Visual Studio [EnvDTE.CodeType][2] class and not a [Type][3] class. The EnvDTE.CodeType has an attributes property you can use to get the display name.

Here is some example code that you can use to get the display name from a CodeType. You can put this code at the end of your custom T4 template (Index.cs.t4).

    <#+
    string GetDisplayName(EnvDTE.CodeType type) {
        if (type != null) {
            foreach (var attribute in type.Attributes.OfType<EnvDTE.CodeAttribute>()) {
                if (attribute.Name == "DisplayName") {
                    return attribute.Value;
                }
            }
        }
        return "";
    }
    #>

Then in your custom T4 template you can replace **viewDataType.Name** with a call to **GetDisplayName()**. I also removed the quotes around "**<#= viewDataType.Name #>**" since the T4 template generates quotes around the result returned from **<#= GetDisplayName(viewDataType) #>**.

    <# var viewDataType = (EnvDTE.CodeType) Model.ViewDataType; #>
    <# if(viewDataType != null) { #>
    @model IEnumerable<<#= viewDataType.FullName #>>
    <# } #>
    @{
        ViewBag.Title = <#= GetDisplayName(viewDataType) #>;
    <# if (!String.IsNullOrEmpty(Model.Layout)) { #>
        Layout = "<#= Model.Layout #>";
    <# } #>
    }

If you then delete your Index.cshtml view and recreate it again with the scaffolder you should get the display name being set in the ViewBag.Title.

    @{
        ViewBag.Title = "Title1";
    }

  [1]: http://mvcscaffolding.codeplex.com/
  [2]: http://msdn.microsoft.com/en-us/library/envdte.codetype%28v=vs.100%29.aspx
  [3]: http://msdn.microsoft.com/en-us/library/system.type%28v=vs.100%29.aspx