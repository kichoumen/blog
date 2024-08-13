---
layout: post
title: "Recovering the sa Password on a Local SQL Server"
date: 2024-08-13
categories: [database-security]
---

![_97fc76f5-80ef-4c63-8486-cf492bf5aa1f (1)](https://github.com/user-attachments/assets/cf28e667-d264-4243-ad74-32c7f2f6f9b4)


Forgetting the `sa` password on your local SQL Server can be stressful, but don’t worry—there’s a way to recover it. Below, I'll walk you through the steps to reset the `sa` password using SQL Server in single-user mode.

### Steps to Recover the sa Password

1. **Stop the SQL Server Service**

   First, you'll need to stop the SQL Server service to start it in single-user mode:

   ```cmd
   net stop MSSQLSERVER
   ```
2. **Start SQL Server in Single-User Mode**

Now, start SQL Server in single-user mode. This mode allows you to connect with SQLCMD and reset the sa password:

```cmd
net start MSSQLSERVER /m
```

3. Connect to SQL Server Using SQLCMD

Open a command prompt and connect to SQL Server using SQLCMD:

```cmd
SQLCMD -Slocalhost -E
```

4. Enable the sa Login

Once connected, enable the sa login:

```sql
ALTER LOGIN sa ENABLE;
GO
```

5. Reset the sa Password

Now, reset the sa password to something strong and secure:

```sql
ALTER LOGIN sa WITH PASSWORD = 'NewSuper1StrongPassword!';
GO
```

6. Create a New Login (Optional)

If you need an additional way to connect as a system administrator, you can create a new login using your Windows username:

```sql
CREATE LOGIN [YOUR-PC-NAME\windows_username] FROM WINDOWS;
GO
```

Then, add the new login to the sysadmin role:

```sql
ALTER SERVER ROLE sysadmin ADD MEMBER [YOUR-PC-NAME\windows_username];
GO
```

7. Restart SQL Server in Normal Mode

Finally, stop SQL Server and restart it normally:

```cmd
net stop MSSQLSERVER
net start MSSQLSERVER
```

And that’s it! You’ve successfully recovered the sa password on your local SQL Server. Remember to keep this password safe and secure, and consider creating an additional login with sysadmin privileges as a backup.
