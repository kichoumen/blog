---
layout: post
title: "The Importance of Updating Statistics in SQL Server"
date: 2024-09-28
categories: database-performance
---

![OIG3 YVf 6e7K](https://github.com/user-attachments/assets/cfb92764-285a-46d0-8e77-a83c28c9d41a)


When working with SQL Server, it's crucial to keep your statistics up to date. [Statistics](https://learn.microsoft.com/en-us/sql/relational-databases/statistics/statistics?view=sql-server-ver16) help the SQL Server query optimizer make informed decisions about the best way to execute a query. Outdated statistics can lead to inefficient query plans and poor performance.

### Detailed Index Information

Here's a query that provides detailed information about the indexes in your database, including when the statistics were last updated:

```sql
USE AdventureWorks
GO

SELECT 
    t.name AS TableName,
    i.name AS IndexName,
    i.type_desc AS IndexType,
    STATS_DATE(i.object_id, i.index_id) AS StatsUpdated,
    p.rows AS RowCount,
    u.user_seeks AS UserSeeks,
    u.user_scans AS UserScans,
    u.user_lookups AS UserLookups,
    u.user_updates AS UserUpdates
FROM 
    sys.indexes i
JOIN 
    sys.tables t ON i.object_id = t.object_id
JOIN 
    sys.partitions p ON i.object_id = p.object_id AND i.index_id = p.index_id
LEFT JOIN 
    sys.dm_db_index_usage_stats u ON i.object_id = u.object_id AND i.index_id = u.index_id
WHERE 
    i.type > 0 -- Exclude heap indexes
ORDER BY 
    StatsUpdated DESC
GO

```

This query will give you a comprehensive view of your indexes, including their types, the number of rows in the tables they belong to, and their usage statistics. Keeping an eye on this information can help you maintain optimal performance in your SQL Server database.

[Creating Statistics](https://learn.microsoft.com/en-us/sql/t-sql/statements/create-statistics-transact-sql?view=sql-server-ver16)

Filtering by Specific Table
If you want to filter the results by a specific table, you can add a WHERE clause to the query. For example, to filter by a table named `SalesOrderDetail`, you can modify the query as follows:


``` SQL
WHERE 
    i.type > 0 AND t.name = 'SalesOrderDetail'
ORDER BY 
    StatsUpdated DESC

```

### Auto-Create and Auto-Update Statistics

SQL Server provides options to automatically create and update statistics:

* **Auto Create Statistics**: It is generally recommended to enable this option as it helps the query optimizer make better decisions by automatically creating statistics on columns used in query predicates, as the default value is `ON`. It creates statistics that name starts with `_WA`. However, in high-transaction environments, enabling this option might cause performance overhead. Monitor the impact on your workload to ensure it does not negatively affect performance.

* **Auto Update Statistics**: Enabling this option is recommended because it ensures that the query optimizer has the most current information by automatically updating statistics when they are out of date. This helps maintain query performance. However, be cautious in high-transaction environments as frequent updates might introduce overhead. It's worth reading about [AUTO_UPDATE_STATISTICS](https://learn.microsoft.com/en-us/sql/t-sql/statements/alter-database-transact-sql-set-options?view=sql-server-ver16#auto_update_statistics) in the SQL Server documentation.

* **Auto Update Statistics Asynchronously**: By default, the value is `OFF`. This option is recommended if you want to allow queries to proceed without waiting for the statistics update to complete. It updates statistics in the background, which can be beneficial in reducing query wait times. However, ensure that your workload can tolerate the slight delay in statistics updates.

These options can be beneficial for maintaining performance without manual effort. However, in high-transaction environments, **automatic updates might cause performance overhead**. It's essential to monitor and decide based on your specific workload. **Nothing beats knowing your own surroundings and tailor-made tuning**. Regularly updating your statistics is a simple yet effective way to ensure your queries run efficiently. By keeping your statistics up to date, you help the SQL Server query optimizer make the best decisions, leading to better performance.

Remember to constantly monitor your database and plan tuning settings based on your workload to maintain optimal performance and avoid potential performance issues.

Peace ✌️

