---
title: "Read Open File in C#"
date: "2013-12-16"
categories:
- "site-updates"
tags:
- "csharp"
aliases:
- "/blog/read-open-files-c/"
blachnietWordPressExport: true
---

_This post is a summary of [this StackOverflow answer](http://stackoverflow.com/a/898017/389899) by [Cheeso](http://stackoverflow.com/users/48082/cheeso)._

To read a file that is currently open (or may be open later) in another process, you must specify a [`FileShare`](http://msdn.microsoft.com/en-us/library/system.io.fileshare.aspx) option. The `FileShare` flag indicates the modes that other processes are allowed to open the file in.

```csharp
using (Stream s = System.IO.File.Open(fullFilePath, 
          FileMode.Open, 
          FileAccess.Read,       // I want to open this file for reading only.
          FileShare.ReadWrite))  // Other processes may specify 
                                 // FileAccess of read or write.
{
}
```

### Reference

- [StackOverflow Answer](http://stackoverflow.com/questions/897796/how-do-i-open-an-already-opened-file-with-a-net-streamreader)
- [FileShare Enumeration (MSDN)](http://msdn.microsoft.com/en-us/library/system.io.fileshare.aspx)
- [CreateFile function (MSDN)](http://msdn.microsoft.com/en-us/library/aa363858%28VS.85%29.aspx)
