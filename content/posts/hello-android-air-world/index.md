---
title: "Hello Android AIR World"
date: "2011-08-20"
categories:
- "site-updates"
tags:
- "actionscript"
- "air"
- "android"
- "flashdevelop"
- "mobile"
- "software-development"
aliases:
- "/blog/hello-android-air-world/"
blachnietWordPressExport: true
---

A while back I wrote my first 'Hello World' Android AIR application but never posted anything about it. Since I have this shiny new site, I figured I’d write up something real quick. If you know me at all, it would come as no surprise that I have not dropped the $700 or so on Flash CS5.5. That means that I did this application, a very simple Pong game, with only FlashDevelop.

## Filling Your Toolbox

I'm not going to walk you through the creation of this game, I'm just going to get you set up to do your own app. To begin you are going to need to get the latest

[FlashDevelop](http://www.flashdevelop.org/community/viewforum.php?f=11) (4.0.0 or later). Note that 4.0.0 and later are the only versions of FlashDevelop that support Android AIR development, and those releases are in Beta at the time of this posting. I never ran into any issues, but this is a heads up that it may not be fully tested yet. You will also have to grab the [Android SDK](http://developer.android.com/sdk/index.html) if you don’t already have it.

## Androids In Your FlashDevelop

Open FlashDevelop, click _Project >> New Project >> AS3 Android App_ to create your new project. FlashDevelop will generate a bunch of batch scripts and other files to get you started. Begin by reading the `AIR_Android_readme.txt` that FlashDevelop generates in your project. This file has a lot of useful instructions on how to get setup including how to generate the self-signed certificate required by AIR and running/debugging on your device. Once you've gone through the steps in that readme, you're ready to start cranking out some ActionScript.

## Code Spelunking

As I said, I’m not going to walk you through the making of my Pong game. This is because the majority of it is just normal AS3 code that is not specific to Android development (certainly not because I'm feeling lazy tonight). If you want, take a look at the code for my app (link at the bottom of this post) and grab the pieces you need to do your own application. You will probably need to regenerate the certificate by running `/bat/CreateCertificate.bat`. Let me know if you have any issues, or find some possible optimizations to my code (particularly in the `TouchEvent` department, it moves pretty slowly when there are multiple touches occurring at once). [Booker](http://www.bhbooker.com/) has been doing some Android AIR development recently so keep an eye out for some sweetness over at his site. He’s actually got Flash CS5.5, so I expect he’ll have some hotter stuff and probably some more useful info. [Download HelloAndroidAir.zip](http://blachniet.com/dropbox/HelloAndroidAir.zip)
