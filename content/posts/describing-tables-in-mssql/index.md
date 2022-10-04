---
title: "Describing Tables in MSSQL"
date: "2013-02-09"
categories:
- "site-updates"
tags:
- "mssql"
- "sql"
- "sql-server"
aliases:
- "/blog/describing-tables-in-mssql/"
blachnietWordPressExport: true
---

There are 2 particularly good stored procedures in SQL Server for getting information about a particular table.

[sp\_columns](http://msdn.microsoft.com/en-us/library/ms176077.aspx) returns detailed information about each of the columns in the table. _Thanks to Vincent Ramdhanie [StackOverflow answer](http://stackoverflow.com/a/319368/389899)_

```
sp_columns @tablename
```

[sp\_help](http://msdn.microsoft.com/en-us/library/aa933429(v=sql.80).aspx) returns detailed information about the entire table including the columns and constraints. _Thanks to Brannon for his [StackOverflow answer](http://stackoverflow.com/a/319366/389899)_

```
sp_help @tablename
```
