---
title: "Remove a Passphrase from a SSH Key"
date: "2013-02-04"
categories:
- "site-updates"
tags:
- "git"
- "github"
- "github-for-windows"
- "ssh"
aliases:
- "/blog/remove-a-passphrase-from-an-ssh-key/"
blachnietWordPressExport: true
---

If you are using [GitHub for Windows](http://windows.github.com/) with non-GitHub repositories, you may have run into the limitation that you cannot sync using an SSH key with a passphrase. One workaround is to push/pull from the shell, but if don't mind removing the passphrase from the key, use the following command.

```
ssh-keygen -p -P "my_old_password" -N “” -f my_key_file_name
```

After you remove the passphrase you should be able to sync from within the [GitHub for Windows](http://windows.github.com/) client.
