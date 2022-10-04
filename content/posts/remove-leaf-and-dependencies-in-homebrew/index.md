---
title: "Remove leaf and dependencies in Homebrew"
date: "2021-08-03"
categories:
- "site-updates"
tags:
- "homebrew"
- "macos"
aliases:
- "/blog/remove-leaf-and-dependencies-in-homebrew/"
blachnietWordPressExport: true
---

A leaf in [Homebrew](https://brew.sh/) is a formula that is not a dependency of another installed formula. You can list the currently installed leaves using the `brew leaves` command. See thoughtbot's [`brew leaves`](https://thoughtbot.com/blog/brew-leaves) post for more information on this command.

Removing a leaf in Homebrew can suddenly introduce many more leaves; the dependencies of the formula you removed. In this post I will show you how to remove a leaf as well as all its dependencies not used by any other formulae. These steps will work in Bash and Zsh.

Create a text file containing regex patterns for all the leaves. We'll use this file later as input to `grep`.

```shell
$ brew leaves | xargs -I {} echo '^{}$' > leaves.txt
```

Edit `leaves.txt`, removing any that you want to uninstall.

Get the list of applications to remove, then remove them. Repeat this process until `brew leaves | grep ...` output is empty.

```shell
$ brew leaves | grep -v --file leaves.txt
gcc
$ brew remove gcc
Uninstalling /usr/local/Cellar/gcc/10.2.0_4... (1,465 files, 339.5MB)
$ brew leaves | grep -v --file leaves.txt
isl
libmpc
$ brew remove isl libmpc
Uninstalling /usr/local/Cellar/isl/0.23... (72 files, 5MB)
Uninstalling /usr/local/Cellar/libmpc/1.2.1... (13 files, 423.6KB)
```

Alternatively, you can perform the list and remove in one step:

```shell
$ brew leaves | grep -v --file leaves.txt | xargs brew remove
Uninstalling /usr/local/Cellar/mpfr/4.1.0... (29 files, 5.1MB)
```
