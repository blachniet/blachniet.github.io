---
title: "TypeScript 0.8.2 on AppHarbor"
date: "2013-02-10"
categories:
- "site-updates"
tags:
- "appharbor"
- "msbuild"
- "typescript"
aliases:
- "/blog/typescript-0-8-2-on-appharbor/"
blachnietWordPressExport: true
---

It doesn't look like [AppHarbor](http://www.appharbor.com) is supporting [TypeScript](http://www.typescriptlang.org) yet, which makes sense considering that TypeScript is in preview. So if you want to use AppHarbor to build your TypeScript files as part of its continuous integration/deployment, you'll have to do a little bit of manual work. Before going through the steps below, make sure you have followed the steps in the [Compile-on-Save](http://typescript.codeplex.com/wikipage?title=Compile-on-Save) TypeScript post.

Copy _C:\\Program Files\\MSBuild\\Microsoft\\Visual Studio\\v11.0\\TypeScript_ and _C:\\Program Files\\Microsoft SDKs\\TypeScript_ into some place in your repository. I copied the contents of both directories into a directory at the top of my repository, _Tools\\Typescript_.

In your copied _Microsoft.TypeScript.targets_, replace

```xml
<Exec Command="tsc $(TypeScriptBuildConfigurations) @(TypeScriptCompile ->'"%(fullpath)"', ' ')" />
```

with

```xml
<Exec Command="..\Tools\TypeScript\tsc.exe $(TypeScriptBuildConfigurations) @(TypeScriptCompile ->'&quot;%(fullpath)&quot;', ' ')" />
```

In your _.csproj_ file, replace

```xml
<Import Project="$(MSBuildExtensionsPath32)\Microsoft\VisualStudio\v$(VisualStudioVersion)\TypeScript\Microsoft.TypeScript.targets" />
```

with

```xml
<Import Project="..\Tools\TypeScript\Microsoft.TypeScript.targets" />
```

If you put the copied files somewhere else in your repository, make sure you modify the paths in the replacement code appropriately. Commit/push your changes and AppHarbor should successfully build your project.
