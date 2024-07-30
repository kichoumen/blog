---
layout: post
title: "Quick Ways to View Locks on SQL Server"
date: 2024-07-30
categories: database-performance
---

![image](https://github.com/user-attachments/assets/d6ca0634-a020-44f3-8634-f0e830306102)


If you're working with SQL Server on Azure (or even on premise), you might sometimes need to check for locks that could be slowing down your system. Fortunately, there are a couple of quick ways to see what's going on.

One straightforward method is to use a query that taps into SQL Server's dynamic management views. The query below helps you identify sessions that are being blocked, which is often a sign of contention issues:

```sql
SELECT session_id,
       blocking_session_id,
       start_time,
       status,
       command,
       DB_NAME(database_id) AS [database],
       wait_type,
       wait_resource,
       wait_time,
       open_transaction_count
FROM sys.dm_exec_requests
WHERE blocking_session_id > 0;
```

This query is like a quick diagnostic tool. It shows the session IDs involved in blocking, the type of wait, and other useful details like the command being executed and the database name. Just run it in your SQL Server Management Studio (SSMS), and you'll get a snapshot of the current blocking situation.

Another handy way to view locks is by using the [Activity Monitor in SSMS](https://learn.microsoft.com/en-us/sql/relational-databases/performance-monitor/activity-monitor?view=sql-server-ver16). This GUI tool provides a more visual way to see not only locks but also processes and performance metrics. It's not as detailed as running custom queries, but it's quick and easy, especially for getting an overview of what's happening on your server.

Both methods are helpful, but they serve slightly different purposes. The query gives you detailed, specific information, which is great for diagnosing specific issues. The Activity Monitor, on the other hand, offers a broader view and is useful for monitoring overall server health.

So, whether you prefer diving into the details with a query or getting a quick overview with Activity Monitor, these tools can help you keep your SQL Server running smoothly.

It's good to mention that there are lots of other ways to monitor locks in your database that you can do using SSMS, SQLcmd or third party tools.

Peace ✌️
