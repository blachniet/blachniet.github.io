---
title: "Referencing EF Resources in a Specific Assembly"
date: "2013-07-13"
categories:
- "site-updates"
tags:
- "c"
- "entity-framework"
aliases:
- "/blog/referencing-ef-resources-in-a-specific-assembly/"
blachnietWordPressExport: true
---

This post and [sample application](https://github.com/blachniet/ef-issue-1377) demonstrate a possible issue with the way that Entity Framework loads the embedded metadata resources (`csdl`, `msl`, `ssdl`) for the entity model.

In my application, all assemblies in some specific directory are discovered and loaded at runtime. Think of it as a sort of plugin model. The application allows for multiple versions of the same assembly to be loaded, and each dll follows the `[AssemblyName].[Version].dll` naming scheme. One of my assemblies that is discovered and loaded at runtime contains an EF entity model and uses it to perform some queries against the database. I will refer to this assembly as `EFContainer`. Whenever I make changes to the database schema, I update the model, and create a new version of `EFContainer`. In order to make sure I load the correct embedded metadata resources, my connection string uses the full assembly name to specify the location of the resoruces as opposed to the default wildcard (`res://*/FooModel.csdl|...`):

```
<add name="FooContext" connectionString="metadata=
    res://EFContainer, Version=2.0.0.0, Culture=neutral, PublicKeyToken=null/FooModel.csdl|
    res://EFContainer, Version=2.0.0.0, Culture=neutral, PublicKeyToken=null/FooModel.ssdl|
    res://EFContainer, Version=2.0.0.0, Culture=neutral, PublicKeyToken=null/FooModel.msl;
    provider=.../>
```

_Herein lies the problem._ The full assembly name in that conenction string does not seem to be honored. Instead it seems that the embedded metadata of the assembly loaded first is used.

To demonstrate this issue, I've created this sample application which uses `EFContainer.1.0.0.0.dll` and `EFContainer.2.0.0.0.dll`. Version 1.0 contains outdated metadata information and is **incompatible** with the current database schema. Version 2.0 contains updated metadata that is **compatible** with the current database schema. The updated schema in 2.0 consists of a new column in the `Bars` table, `Rating`, which is an `INT` with a `NOT NULL` constraint. As we would expect, using the metadata in version 1.0 throws an exception because the `Rating` column does not exist in the metadata and it does not allow for null values.

Since I am including the full assembly name of version 2.0 in my connection string, I would expect that the resources from the version 2.0 assembly would be used. However, it seems like it actually just uses the resources from whichever assembly was loaded first. You can test this out for yourslf in the `EFMultiSchemaVersionTest`'s `Program.cs`. If you run it as-is, it will throw an exception indicating that `Rating` does not allow for null values. If you uncomment the line containing `filesToLoad = filesToLoad.Reverse().ToArray();`, causing the v2.0 assembly to be loaded before the v1.0, the application will complete without error.

In the meantime I have resorted to setting the **Metadata Artifact Processing** to **Copy to Output Directory**, and putting those metadata files in a public location that the assemblies can access at runtime. If anyone notices something that I'm missing, please let me know. I would prefer to embed those resources in the assembly.

- [CodePlex Issue](http://entityframework.codeplex.com/workitem/1377)
- [Example Application on GitHub](https://github.com/blachniet/ef-issue-1377)
