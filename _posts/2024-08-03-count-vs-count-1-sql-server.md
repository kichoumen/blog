---
layout: post
title: "COUNT(*) vs COUNT(1) in SQL Server"
date: 2024-08-03
categories: [database-performance]
---

![Screenshot 2024-08-04 104729](https://github.com/user-attachments/assets/e185e73f-8b28-4d27-a9a2-6270131b794c)


Ever wondered if you should use `COUNT(*)` or `COUNT(1)` in your SQL queries? Let's clear up the confusion, especially when it comes to performance.

Both `COUNT(*)` and `COUNT(1)` are used to count rows in a table. The difference? `COUNT(*)` counts all rows, including NULLs, while `COUNT(1)` counts by evaluating the constant `1` for each row. Here’s how they look using the AdventureWorks database:

```sql
SELECT COUNT(*) FROM Person;

SELECT COUNT(1) FROM Person;
```

You might think COUNT(1) is faster since it doesn't check for NULLs. However, in SQL Server, there's no performance difference. The query optimizer treats them the same, generating identical execution plans. Both queries will perform equally efficiently.

When you check the execution plans to see this in action, running the above queries in SQL Server Management Studio (SSMS) will show identical plans for both. This means SQL Server is smart enough to know that both forms are just counting rows.

So, which one should you use? It's mostly personal preference. Some folks prefer COUNT(1) because it feels more explicit, while others stick with COUNT(*) because it's more common and universally understood.

In short, there's no performance gain in choosing COUNT(1) over COUNT(*) in SQL Server. They both do the job just as well, so use whichever you’re more comfortable with.

Peace ✌️
