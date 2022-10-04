---
title: "Force Bundling Optimizations"
date: "2013-12-16"
categories:
- "site-updates"
tags:
- "angularjs"
- "asp-net-mvc"
- "csharp"
aliases:
- "/blog/force-bundling-optimizations/"
blachnietWordPressExport: true
coverImage: "sound3.png"
---

Use the following code in the `BundleConfig.cs` of your ASP.NET MVC application to force bundling and minification.

```
// BundleConfig::RegisterBundles
BundleTable.EnableOptimizations = true;
```

## Why?

_Why would you do this?_ Sometimes bundling and minification causes problems, such as in [AngularJS](http://docs.angularjs.org/tutorial/step_05#controller_a-note-on-minification). You will want to find these problems before publishing, so just enable this line every once in a while to make sure everything still works as expected with the optimizations.
