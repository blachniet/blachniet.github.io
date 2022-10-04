---
title: "Assertions in Moq Callbacks"
date: "2015-02-09"
categories:
- "site-updates"
aliases:
- "/blog/assertions-in-moq-callbacks/"
blachnietWordPressExport: true
---

I learned an important lesson today: **do not _assert_ inside of [Moq](https://github.com/Moq/moq4) callbacks**. In the code below, even if the `Assert.AreEqual` fails, it will not cause the unit test to fail.

```
moqObj.Setup(m => m.Doit(It.IsAny<string>())).Callback<string>((s) => 
{
    Assert.AreEqual("foo", s);
});
// Insert test code that invokes `Doit`
```

You could use [`Verify`](https://github.com/Moq/moq4/wiki/Quickstart#verification) instead, but it's not always ideal, particularly when trying to step through the assertions while debugging tests. Instead, you can take this approach suggested by [Thomas Ardal](http://thomasardal.com/using-moq-callbacks-as-verify/).

```
string actual = null;
moqObj.Setup(m => m.Doit(It.IsAny<string>())).Callback<string>((s) => 
{
    actual = s;
});
// Insert test code that invokes `Doit`
Assert.AreEqual("foo", actual);
```
