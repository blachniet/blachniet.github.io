---
title: "Host Git Repositories on a Windows Share"
date: "2013-06-24"
categories:
- "site-updates"
tags:
- "git"
- "windows"
aliases:
- "/blog/host-git-repositories-on-a-windows-share/"
blachnietWordPressExport: true
---

I usually use [BitBucket](http://bitbucket.org) or [GitHub](http://github.com) to host my git repositories, but sometimes I need to host them on some internal network. In this post I will show you how to host those repositories on a Windows share.

I'm going to cover two scenarios: 1) _You already have a git repository_ 2) _You are starting fresh with a brand new repository_. For both of these tracks I'm going to assume that you already have a Windows share set up on some remote computer, and that you have the read and write permissions on that share. To see more detailed information about hosting git on a server, check out section [4.2 Git on the Server - Getting Git on a Server](http://git-scm.com/book/ch4-2.html) of the [Pro Git](http://git-scm.com/book) book.

## I Already Have a Repository

Clone your repo (`my_project`) into a new bare repository (`my_project.git`):

```sh
> git clone --bare my_project my_project.git
```

Move the bare repository to the Windows share.

Add a remote to `my_project` that points to `my_project.git`. You can name your remote something other than `origin` if you would like. _Note that forward slashes are used, not backslashes_.

```sh
> git remote add origin //{ServerNameOrIp}/{ShareName}/my_project.git  
```

You're all set up. You can now `push` and `pull` from that remote repository. If you need to clone that repo later, just use

```sh
> git clone //{ServerNameOrIp}/{ShareName}/my_project.git`
```

## I Don't Have a Repository Yet, I'm Starting Fresh

Create a new bare repository.

```sh
> mkdir my_project.git > cd my_project.git > git init --bare
```

Move the bare repository to the Windows share.

Clone the repository. _Note that forward slashes are used, not backslashes_.

```sh
> git clone //{ServerNameOrIp}/{ShareName}/my_project.git`
```

You're all set up. You can now `push` and `pull` from that remote repository.
