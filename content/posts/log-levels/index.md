---
title: "Reevaluate Your Log Levels"
date: "2015-09-12"
categories:
- "site-updates"
tags:
- "logging"
aliases:
- "/blog/log-levels/"
blachnietWordPressExport: true
coverImage: "13661764254_55f0a635b8_k.jpg"
---

Constantly reevaluate the logging level applied to log events. Your typing was not directed by angels and your code was not ordained by God when you selected **warning** as the level for your log event. That code is up for change just like the rest.

Too often I see piles of log events with poorly assigned log levels. If there’s an **error** event flooding your logs that you have no intention to fix, change the logging level. If life can move on normally with that log event, it’s not worthy of the **error** level.

The biggest problem with incorrectly assigned logging levels is that it can desensitize developers and operations to the importance of those events. When an **error** occurs, you want people to be surprised. You want them to investigate and resolve the issue quickly. If they have a constant flow of the same **error** events, the error level is going to lose its oomph.

So how do you choose a logging level? That requires some coordination with your team. The team needs to decide what logging levels are available, what they mean, and when they should be used.

I don’t want a developer to have to think long and hard about the level for an event. I prefer to keep it as simple as possible. I do this by encouraging devs to feel free to change the logging level in the future and by providing and small set of common logging levels with clear distinctions. Here’s the levels I like to use the most:

- **FATAL** - Something's very wrong. So wrong, in fact, that we had to drop everything and bail immediately. A human needs to investigate this as soon as possible. If the application is a one-off task that produces output, the output is likely incomplete, and should not be trusted. If the application is a service or interactive experience, it’s probably dead.
- **ERROR** - Again, there’s something very wrong and a human needs to investigate this as soon as possible. However, we were able to recover and continue. If the application is a one-off task producing output, you can probably glean some useful information from the output, but it is probably incomplete.
- **WARN** - There’s something strange going on. Maybe we’re seeing data that we’ve never seen before. Maybe the user did something strange. Maybe a new version of the web API we use is available. Whatever it was, our application is still in a good state and is still running fine. However, a human should investigate this soon and resolve the warning.
- **INFO** - This level is for everything else. Application flow, statistics, metrics, and other general information.

You might have noticed that I left off a few of the common logging levels such as **VERBOSE**, **DEBUG** and **TRACE**. With [well structured log events and good analysis tools](http://blachniet.com/blog/structured-logging-serilog-elk/), events with that kind of information can be collapsed into the **INFO** level and still be easily filtered and distinguished. Again, a major goal with this level structure is to require as little thought and consideration as possible when choosing a level.

[**I'm soooo nice!!** photo by clement127 used under CC](https://flic.kr/p/mPf7xy)
