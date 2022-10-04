---
title: "Use a graphical diff tool with Fossil"
date: "2021-09-18"
categories:
- "site-updates"
tags:
- "fossil"
aliases:
- "/blog/use-a-graphical-diff-tool-with-fossil/"
blachnietWordPressExport: true
---

You can configure [Fossil](https://fossil-scm.org/)'s `gdiff` command to launch your favorite graphical diff tool. Below are examples configuring some popular diff tools.

All the examples below include the `--global` option. If you want to configure this setting only for the current repository, omit that option.

[Beyond Compare](https://scootersoftware.com/):

```shell
$ fossil settings --global gdiff 'bcomp'
```

[Git](https://git-scm.com/):

```shell
$ fossil settings --global gdiff 'code --diff --wait'
```

`vimdiff`:

```shell
$ fossil settings --global gdiff 'vimdiff'
```

[Visual Studio Code](https://code.visualstudio.com/):

```shell
$ fossil settings --global gdiff 'code --diff --wait'
```
