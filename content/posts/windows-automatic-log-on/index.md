---
title: "Windows Automatic Log On"
date: "2013-09-23"
categories:
- "site-updates"
tags:
- "security"
- "windows"
aliases:
- "/blog/windows-automatic-log-on/"
blachnietWordPressExport: true
---

I have a few applications installed on my home server that are user-level applications, meaning that a user must be logged on in order for the application to start. I don't want to have to manually log on ever time my server restarts, so I set up automatic log on for the server. Here's how to do it.

- Press `Windows Key` + `R` to launch the run dialog.
- Type in `netplwiz` and click `OK`. ![Launch netplwiz](images/autologon-1.jpg)
- In the dialog disable _Users must enter a username and password to use this computer_.  
    ![Disable Users must enter a username and password to use this computer](images/autologon-2.jpg)
- Click _Apply_ and a dialog will appear. Enter the user name and password of the account that you would like to be automatically logged in.  
    ![Insert automatic log on credentials.](images/autologon-3.png)

That's all there is to it. Having an automatic log on does not change the need to log on when using remote desktop to log on. I would suggest that you automatically log on to a standard account, not an admin account. That way if someone does sit down in front of the computer and start messing with things they shouldn't, they can only do so much damage.
