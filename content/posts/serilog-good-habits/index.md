---
title: "Good Habits for Structured Logging with Serilog"
date: "2015-03-03"
categories:
- "site-updates"
tags:
- "logging"
- "serilog"
aliases:
- "/blog/serilog-good-habits/"
blachnietWordPressExport: true
---

Below is a collection of good habits I've identified in my recent experiences with structured logging. The code examples are specific to [Serilog](http://serilog.net/), but the ideas can be applied to other structured logging tool sets (my stack of choice for .NET projects is [Serilog, Elasticsearch and Kibana](http://blachniet.com/blog/structured-logging-serilog-elk/)). Got your own structured logging practices to contribute? [Share them on Twitter!](https://twitter.com/share?text=@blachniet)

### EventID

When you create you log events, include an `EventID` property to uniquely identify the event. This will make it easier to build queries when analyzing your logs later.

I like to make my event ids stand out in the message, so I generally wrap them in `<>` or something similar. I also include the `:l` modifier to leave out the double quotes in the message.

```
log.Information(
"<{EventID:l}>  {Handler} - {ElapsedMilliseconds}ms", 
"HandlerCompleted", handler.GetType().Name, stopwatch.ElapsedMilliseconds);

/// Message:
///     <HandlerCompleted> "IndexHandler" - 250ms   
```

### SourceContext

Always include the code entity responsible for creating the log event. This will help you narrow down where problems are coming from, particularly if you have multiple code entities that produce the same `EventID`. Serilog provides an easy way to do this with the `ForContext<T>()` method on loggers which adds a `SourceContext` property on the log event.

```
var log = baseLogger.ForContext<IndexHandler>();
log.Error("<{EventID:l}>", "DbConnErr");

// JSON:
// { 
//  "EventID": "DbConnErr", 
//  "SourceContext": "IndexHandler" 
// }
```

### ContextID

If you have a stream of log events that are related (e.g. multiple log events from a single HTTP transaction), include a property to link them all together. I generally call mine `ContextID`. When you are analyzing your logs later and want to look at a single transaction, you can filter by this property. You can use the `ForContext()` method on loggers to attach this property to log events without including it in the log message.

```
log.ForContext("ContextID", transactionID)
.Information("<{EventID:l}>", "TransactionStarted");

// JSON:
// { 
//  "EventID": "TransactionStarted", 
//  "SourceContext": "IndexHandler",
//  "ContextID": "e6f82c53-557f-448d-ab75-f05e344140bc"
// }
```

### Terse Messages

Try to keep your log messages brief. A good log event message gives a very short summary of a log event, not extended details about the event. Most of my log event messages only include an `EventID` and one or two scalar properties.

I'm not saying to exclude details from log events, just don't include them in the message. For example, don't [destructure](https://github.com/serilog/serilog/wiki/Structured-Data#preserving-object-structure) big objects in your messages. It just makes the message hard to read. Your log events are ideally going to a destination that can handle structured events, [like Elasticsearch](http://blachniet.com/blog/structured-logging-serilog-elk/), so you can still query on those details even when they are not included in the message.

```
var user = new User{
  Name = "John",
  Email = "john.doe@example.com",
  Password = "p1nkBunni3s"
};

// Do This
// Message: <NewUser> "john.doe@example.com"
log.ForContext("User", user, destructureObjects=true)
   .Information("<{EventID:l}> {Email}",
                "NewUser", user.Email);

// Don't Do This
// Message: <NewUser> { "User": {"Name": "John", "Email": "john.doe@example.com", "Password": "p1nkBunni3s"}}
log.Information("<{EventID:l}> {User}", 
                "NewUser", user);
```

### Extensions

Add some extension methods to make logging even easier. For example, I added extension methods that allowed me to include the `EventID` in the log message without having to manually add it to the message template for each event.

```
public static void InformationEvent(this ILogger logger, string eventID, 
    string messageTemplate, params object[] propertyValues)
{
    var allProps = new object[]{ eventID }.Concat(propertyValues).ToArray();
    logger.Information("<{EventID:l}> " + messageTemplate, props);
}

// Example Usage:
log.InformationEvent("NewUser", "{Email}", user.Email);
```

I also like to create an extension method to wrap calls to `ForContext`. I like the `With` terminology used by [logrus](https://github.com/Sirupsen/logrus), so I generally create an extension method with the same name which destructures the given value by default (`ForContext` does not destructure by default).

```
public static ILogger With(this ILogger logger, string propertyName, object value)
{
    return logger.ForContext(propertyName, value, destructureObjects=true);
}

// Example Usage:
log.With("User", user)
   .InformationEvent("NewUser", "{Email}", user.email)
```

### Dispose Sinks

If you are using a sink that derives from `PeriodicBatchingSink`, then you may want to ensure that you `Dispose` of your sink explicitly. Disposing the sink is the only way I've found to explicitly flush these sinks. If the sink has events in it that have not been sent when the application exits, those events will be lost if you don't `Dispose` of it.
