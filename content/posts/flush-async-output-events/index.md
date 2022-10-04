---
title: "Flush Async Output Events"
date: "2015-05-19"
categories:
- "site-updates"
tags:
- "csharp"
aliases:
- "/blog/flush-async-output-events/"
blachnietWordPressExport: true
---

The [`System.Diagnostics.Process`][1] class provides the options to retrieve all output from an external process after it has completed, or to retrieve events containing the output as the execution occurs. The option to receive asynchronous output events is useful in scenarios where you need to provide user feedback or log the output when timeouts occur.

When receiving asynchronous output events, you must ensure that the external process has completed when you want to make sure that you get all the output. However, waiting for [`HasExited`](https://msdn.microsoft.com/en-us/library/system.diagnostics.process.hasexited(v=vs.110).aspx) to be true is not enough. In order to ensure that all processing has completed, including the handling of asynchronous output events, you must call [`Process.WaitForExit()`](https://msdn.microsoft.com/en-us/library/fb4aw7b8%28v=vs.110%29.aspx). You need to call the overload taking no arguments, even if you previously called [`Process.WaitForExit(Int32)`](https://msdn.microsoft.com/en-us/library/ty0d8k56(v=vs.110).aspx) overload. The [`Process.WaitForExit(Int32)`](https://msdn.microsoft.com/en-us/library/ty0d8k56(v=vs.110).aspx) docs state:

> To ensure that asynchronous event handling has been completed, call the WaitForExit() overload that takes no parameter after receiving a true from this overload.

Below is a simple example that uses [`Process.WaitForExit(Int32)`](https://msdn.microsoft.com/en-us/library/ty0d8k56(v=vs.110).aspx) to enforce timeouts and [`Process.WaitForExit()`](https://msdn.microsoft.com/en-us/library/fb4aw7b8%28v=vs.110%29.aspx) to flush the output events when the process completes.

```csharp
static void DoExternalThing()
{
    var proc = new Process
    {
        EnableRaisingEvents = true,
        StartInfo = new ProcessStartInfo
        {
            FileName = "foo.exe",
            UseShellExecute = false,
            RedirectStandardError = true,
            RedirectStandardOutput = true,
        },
    };
    proc.ErrorDataReceived += proc_ErrorDataReceived;
    proc.OutputDataReceived += proc_OutputDataReceived;

    proc.Start();
    proc.BeginErrorReadLine();
    proc.BeginOutputReadLine();

    // Timeout after 10 seconds
    if (!proc.WaitForExit(10000))
    {
        Console.WriteLine("Operation timed out");
        return;
    }

    // Flush async events
    proc.WaitForExit();
    Console.WriteLine("Operation complete.");
}
```

[1]: https://msdn.microsoft.com/en-us/library/System.Diagnostics.Process(v=vs.110).aspx
