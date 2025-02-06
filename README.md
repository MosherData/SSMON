

# Quick Check

## 1. Max Server Memory
**Why:** Helps prevent SQL Server from consuming all available memory, ensuring that the OS and other applications have enough memory to function properly.
**Setting to Check:**

## 2. Max Degree of Parallelism (MAXDOP)
**Why:** Controls how many CPUs SQL Server can use for a single query. Setting this too high or too low can affect query performance.
**Best Practice:** Set to 2 and work your way up.

## 3. Cost Threshold for Parallelism
**Why:** Determines the cost threshold (in terms of estimated CPU cost) for when a parallel plan is used. A higher value forces queries to use serial plans unless they are more expensive.
**Best Practice:** Default is 5; you might want to adjust this based on workload type.

## 4. SQL Server Trace Flags
**Why:** Trace flags control specific behavior in SQL Server. Common trace flags for troubleshooting and performance tuning include 1204, 1222 (deadlock logs), and 1807 (filegroup information).

## 5. Recovery Model
**Why:** Controls how transactions are logged and determines the backup strategy. The recovery model impacts both performance and the ability to recover from a disaster.
**Best Practice:** Choose between Simple, Full, or Bulk-Logged based on your needs.
**Setting to Check:**
```sql
SELECT name, recovery_model_desc FROM sys.databases;
```

## 6. Database Auto Growth Settings
**Why:** Auto-growth settings define how database files grow. Setting appropriate auto-growth increments can improve performance and avoid fragmentation.
**Best Practice:** Avoid the default 1 MB increment; instead, set auto-growth based on a reasonable percentage or fixed size that fits your growth pattern.
**Setting to Check:**
```sql
SELECT name, growth FROM sys.master_files WHERE database_id = DB_ID('YourDatabase');
```

## 7. TempDB Configuration
**Why:** TempDB plays a critical role in SQL Server performance. Ensuring it has enough space and is properly configured is essential.
**Best Practice:** TempDB should be on its own dedicated disk, and the number of files should be equal to the number of logical processors (up to 8 files).
**Setting to Check:**
```sql
EXEC sp_helpfile;
```

## 8. SQL Server Agent Configuration
**Why:** SQL Server Agent is essential for automating tasks such as backups and maintenance jobs. You want to ensure it’s running and configured properly.
**Setting to Check:**
```sql
SELECT * FROM msdb.dbo.sysjobs;
```

## 9. Database Backup and Maintenance Plans
**Why:** A robust backup and maintenance plan ensures data protection and system stability.

Ensure that regular index maintenance and integrity checks are in place.

## 10. Audit and Security Settings
**Why:** Proper auditing and security settings protect your SQL Server instance from unauthorized access.
**Key Settings:**
**Check for login auditing:**
```sql
SELECT is_login_audit_enabled FROM sys.server_principals WHERE type = 'S';
```
Ensure that the sa account is disabled or protected.

## 11. Check for Blocked Processes
**Why:** Blocking can significantly affect performance. You need to monitor and address any blocking issues proactively.
**Setting to Check:**
```sql
EXEC sp_whoisactive;
```
**OR**

```sql
SELECT blocking_session_id, session_id, wait_type, wait_time, wait_resource FROM sys.dm_exec_requests WHERE blocking_session_id <> 0;
```

## 12. SQL Server Error Logs
**Why:** Regularly monitoring the error logs for issues such as failed login attempts, performance degradation, or hardware failures is vital.
**Setting to Check:**
```sql
EXEC xp_readerrorlog;
```

## 13. Index Fragmentation
**Why:** Fragmented indexes can slow down query performance. Ensure that index fragmentation is addressed regularly.
**Setting to Check:**
```sql
SELECT * FROM sys.dm_db_index_physical_stats (NULL, NULL, NULL, NULL, 'DETAILED');
```

## 14. Database Compatibility Level
**Why:** The compatibility level impacts how SQL Server will optimize queries. Ensure that databases are using the correct compatibility level for your version of SQL Server.
**Setting to Check:**
```sql
SELECT name, compatibility_level FROM sys.databases;
```

## 15. Query Store Configuration
**Why:** Query Store helps track query performance over time and can be used for troubleshooting.
**Setting to Check:**
```sql
SELECT name, is_query_store_on FROM sys.databases;
```

## 16. Server Collation
**Why:** The collation determines how string comparison is done at the server level, which affects sorting, querying, and indexing.
**Setting to Check:**
```sql
SELECT SERVERPROPERTY('Collation');
```

## 17. Transparent Data Encryption (TDE)
**Why:** TDE encrypts your entire database and protects your data at rest. It’s essential for compliance and security.
**Setting to Check:**
```sql
SELECT database_id, name, encryption_state FROM sys.dm_database_encryption_keys;
```

## 18. SQL Server Licensing
**Why:** Ensure that your SQL Server is licensed properly, especially in virtual environments where licensing can be more complex.
**Setting to Check:**
```sql
SELECT SERVERPROPERTY('ProductVersion'), SERVERPROPERTY('Edition');
```

## 19. Ensure that SQL Server's Latest Cumulative Update (CU) is Installed
**Why:** Keeping SQL Server up-to-date with the latest cumulative updates ensures that performance, security, and bug fixes are applied.
**Setting to Check:**
```sql
SELECT SERVERPROPERTY('ProductVersion');
```

## 20. TempDB File Sizing
**Why:** Improper sizing of TempDB files can lead to performance issues. Ensure TempDB is sized correctly and grows appropriately.
**Setting to Check:**
```sql
EXEC sp_helpfile;
```
```
