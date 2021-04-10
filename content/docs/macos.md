---
title: "macOS"
weight: 1
# bookFlatSection: false
# bookToc: true
# bookHidden: false
# bookCollapseSection: false
# bookComments: true
---

# macOS

## Homebrew

### Remove leaf applications and dependencies

Create a text file containing regex patters for all the leaves. We'll use this
file later as input to `grep`.

```bash
brew leaves | xargs -I {} echo "^{}$" > leaves.txt
```

Edit `leaves.txt`, removing any that you want to uninstall.

Get the list of applications to remove, then remove them. Repeat this process
until `brew leaves | grep ...` output is empty.

```bash
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

```bash
$ brew leaves | grep -v --file leaves.txt | xargs brew remove
Uninstalling /usr/local/Cellar/mpfr/4.1.0... (29 files, 5.1MB)
```
