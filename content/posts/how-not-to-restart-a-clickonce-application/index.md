---
title: "How Not To Restart A ClickOnce Application"
date: "2012-10-20"
categories:
- "site-updates"
tags:
- "c"
- "clickonce"
- "wpf"
aliases:
- "/blog/how-not-to-restart-a-clickonce-application/"
blachnietWordPressExport: true
---

Normally if you need to restart a WPF application programatically, you would use the following code:

```
private void Restart() 
{
    System.Diagnostics.Process.Start(
        Application.ResourceAssembly.Location); 
    Application.Current.Shutdown(); 
}
```

If you have a **ClickOnce** WPF application, you do not want to do this. In order to understand why, you need to understand how a **ClickOnce** application is normally launched.

The shortcut you click on in the Start menu is not a normal shortcut to an executable. It is actually an _appref-ms_ file, which defines the entry point and location of the application. When you launch using this file, all the parameters in [`ApplicationDeployment`](http://msdn.microsoft.com/en-us/library/system.deployment.application.applicationdeployment.aspx) are correctly initialized, and [`ApplicationDeployment.IsNetworkDeployed`](http://msdn.microsoft.com/en-us/library/system.deployment.application.applicationdeployment.isnetworkdeployed.aspx) is set to `true`.

However, if you use the code above, you launch using the application's entry executable instead of the _appref-ms_. This will cause [`ApplicationDeployment.IsNetworkDeployed`](http://msdn.microsoft.com/en-us/library/system.deployment.application.applicationdeployment.isnetworkdeployed.aspx) to be `false`, and the properties will not be initialized as if they were a **ClickOnce** deployment.

According to Rob Relyea's post, [Application.Restart() for WPF](http://robrelyea.wordpress.com/2007/07/24/application-restart-for-wpf/), you can get the desired functionality with the following code. I have not had a chance to verify that this works as expected, but I will update this post as soon as I do.

**UPDATE:** I've verified that the following code does work properly. Sadly, you will need a reference to the `System.Windows.Forms.dll`.

```
private void Restart()
{ 
    // from System.Windows.Forms.dll
    System.Windows.Forms.Application.Restart();
    Application.Current.Shutdown();
}
```
