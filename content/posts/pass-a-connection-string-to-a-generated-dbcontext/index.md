---
title: "Pass a Connection String to a Generated DbContext"
date: "2013-02-03"
categories:
- "site-updates"
tags:
- "c"
- "dbcontext"
- "entity-framework"
- "visual-studio-2012"
aliases:
- "/blog/pass-a-connection-string-to-a-generated-dbcontext/"
blachnietWordPressExport: true
---

The default `DbContext` generated with your Entity Data Model has one constructor that takes no arguments. If you need to build the connection string programatically and pass it to the `DbContext`, you will have to add your own constructor. However, if you do this in the generated file, `{ModelName}.Context.cs`, you will lose your changes the next time the `{ModelName}.Context.tt` script is run.

The solution is to edit the script, `{ModelName}.Context.tt`. Just add the following code after the code to generate the default constructor (around line 71). Run the script (_Ctrl+S_), and verify that the new constructor exists in `{ModelName}.Context.cs`.

```
    public <#=code.Escape(container)#>(string nameOrConnectionString)
    : base(nameOrConnectionString)
    {
<#
if (!loader.IsLazyLoadingEnabled(container))
{
#>
    this.Configuration.LazyLoadingEnabled = false;
<#
}
#>
    }
```
