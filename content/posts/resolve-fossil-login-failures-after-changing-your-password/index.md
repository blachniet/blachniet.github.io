---
title: "Resolve Fossil login failures after changing your password"
date: "2021-09-06"
categories:
- "site-updates"
tags:
- "fossil"
aliases:
- "/blog/resolve-fossil-login-failures-after-changing-your-password/"
blachnietWordPressExport: true
---

After changing your password on a remote [Fossil](https://fossil-scm.org/) repository, you will receive login failures when trying to sync your local repository.

```shell
$ fossil sync
Sync with https://blachniet@example.com/test
Round-trips: 1   Artifacts sent: 0  received: 0
Error: login failed
Round-trips: 1   Artifacts sent: 0  received: 0
Sync done, sent: 420  received: 285  ip: 192.168.0.10

$ fossil commit
Autosync:  https://blachniet@example.com/test
Round-trips: 1   Artifacts sent: 0  received: 0
Error: login failed
Round-trips: 1   Artifacts sent: 0  received: 0
Pull done, sent: 446  received: 287  ip: 192.168.0.10
Autosync failed.
continue in spite of sync failure (y/N)?
```

The error occurs because your local repository is still trying to use your old password to sync with the remote.

To update the password in your local repository, run `fossil sync <url>`. When provided with a URL, this command will prompt you for a password. Enter your new password and instruct Fossil to save it.

```shell
$ fossil sync https://blachniet@example.com/test
password for blachniet:
remember password (Y/n)? Y
Round-trips: 1   Artifacts sent: 0  received: 0
Sync done, sent: 456  received: 305  ip: 192.168.0.10
```

Future syncs will use the new password.
