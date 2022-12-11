---
title: "Flash Data Storage Services : Part 2"
date: "2011-09-06"
categories:
- "site-updates"
tags:
- "actionscript"
- "amfphp"
- "easyphp"
- "flashdevelop"
- "mysql"
- "php"
- "software-development"
- "sql"
aliases:
- "/blog/flash-data-storage-services-part-2/"
blachnietWordPressExport: true
---

In my last post we set up our _EasyPHP_ server and created a _MySQL_ database for our event manager. In this post we are going to create the _Amfphp_ service to create, delete, and get events from the database. We are also going to create an ActionScript 3 class to talk to the PHP service.

## Creating the Service

Your event service will be contained within a single PHP file in the `Amfphp\Services\` directory. You can get to this by right clicking on the _EasyPHP_ icon in your notification area and selecting **Explore**. Navigate to the `Amfphp\Services\` directory and create your new PHP file, `EventService.php`. Instead of pasting all the code for this class in this post, I have uploaded the code [here](http://blachniet.com/dropbox/FlashDataStorageServices/EventService.zip). The service is made up of a single PHP class, EventService. The first thing you will see in this class is helper functions to open and close the connection to the database.

```php
private function openDB(){ 
    // Open the database connection.
    $db = mysql_connect("localhost", "root", "yourPassword"); 
    //Select the event database. 
    mysql_select_db("event_manager_db", $db); 
    return $db; 
} 
private function closeDB($db){ 
    mysql_close($db); 
}
```

You will then see the code for our three service functions, `CreateEvent`, `DeleteEvent`, and `GetEvents`. Notice that `opeDB()` is always called before any SQL commands are issued and that `closeDB()` is called after the SQL connection is no longer needed. Most everything in between is normal MySQL/PHP interaction. `CreateEvent()` takes in the name and location of the event. These are then used in the SQL INSERT statement. `mysql_query` returns true/false for INSERT statements, so we build our success/failure return based on this value. `DeleteEvent()` is fairly straightforward as well. If you remember from the last post, we made the name of events `UNIQUE` in the database, so the delete function uses this to delete the proper event. `mysql_query` returns true/false for SQL DELETE queries as well. `GetEvents()` is a little different in that the SQL SELECT query returns row data instead of a true/false value. We iterate through this row data and build an array of event data to return. It is important that you make sure to release the row data after copying it into your array by calling `mysql_free_result($result);`.

```php
while ($row = mysql_fetch_assoc($result)){ 
    array_push($event_array, $row); 
} 
mysql_free_result($result);
```

After your service is complete you should be able to go to [http://127.0.0.1:8888/Amfphp/](http://127.0.0.1:8888/Amfphp/) and see your EventService. Here you can test the service by calling the 3 public functions implemented in the service class.

## ActionScript Data Manager

We can now create an ActionScript class that will handle communication between the PHP service and the rest of the ActionScript. I have posted the source for this project [here](http://blachniet.com/dropbox/FlashDataStorageServices/EventManager.zip), which includes the _FlashDevelop_ project file, an EventDataManager class, and a test document class (Main.as). You should be able to open this in _FlashDevelop_ and compile/run it. The `EventDataManager` extends the Adobe implemented `NetConnection` class, which has two functions we are going to use, `connect()` and `call()`. We call `connect` with the address of the server (with the "/Amfphp/" directory tacked on) to establish a connection with the PHP service. The functions in the EventDataManager class are very simple pass-throughs to the `call` method. For example, this function calls the `CreateEvent` function in the EventService:

```actionscript
public function CreateEvent(name:String, 
    location:String, cbCreateEvent:Responder=null):void{
    call("EventService/CreateEvent", cbCreateEvent, name, location); 
}
```

The Responder is created from a function which will be called after the response from the service is received. Take a look at Main.as to get an idea of how to use the data manager class. If you have any questions or notice any errors, please let me know.

## Links/Reference

- [MySQL Command Quick Reference](http://www.pantz.org/software/mysql/mysqlcommands.html)
- [PHP MySQL Reference](http://www.php.net/manual/en/ref.mysql.php)
- [Amfphp Home Page](http://projects.silexlabs.org/?/amfphp#/php.remoting/what.is.amfphp)

## Files

- [Event Service](http://blachniet.com/dropbox/FlashDataStorageServices/EventService.zip)
- [Event Manager](http://blachniet.com/dropbox/FlashDataStorageServices/EventManager.zip)
