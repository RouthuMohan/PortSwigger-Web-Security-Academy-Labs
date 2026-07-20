# Lab 01 - SQL Injection Vulnerability in WHERE Clause Allowing Retrieval of Hidden Data

## Lab Information

|    Field       |   Details   |
|----------------|-------------|
| Platform       | PortSwigger Web Security Academy |
| Category       | SQL Injection |
| Lab Name       | SQL Injection Vulnerability in WHERE Clause Allowing Retrieval of Hidden Data |
| Difficulty     | Apprentice |
| Status         | ✅ Solved |
| Testing Method | Manual Testing |

---

# Objective

Identify a SQL Injection vulnerability in the `category` parameter and retrieve hidden (unreleased) products by modifying the SQL query.

---

# Testing Summary

|       Item           | Value |
|----------------------|--------------|
| Vulnerable Parameter | `category` |
| Injection Type       | Boolean-based SQL Injection |
| Payload Used         | `' OR 1=1--` |
| Result               | Hidden products displayed |
| Tool Used            | Browser (Manual Testing) |

---

# Vulnerability Overview

The application filters products using a SQL query similar to:

```sql
SELECT * FROM products
WHERE category='Accessories'
AND released=1;
```

The application directly inserts user input into the SQL query without proper validation or parameterized queries.

This allows an attacker to modify the SQL query and retrieve unauthorized data.

---

# Manual Testing

## Step 1 - Identify the Input Parameter

Target URL:

```http
/filter?category=Accessories
```

The `category` parameter is reflected in the SQL query and is the target for testing.

---

## Step 2 - Test for SQL Injection

### Payload

```sql
'
```

### Purpose

A single quote (`'`) is used to determine whether user input is interpreted as part of the SQL query.

### Observation

The application's response changed, indicating that the input may be vulnerable to SQL Injection.

---

## Step 3 - Exploit the Vulnerability

### Payload

```sql
' OR 1=1--
```

### Purpose

This payload modifies the SQL query so that the WHERE condition always evaluates to TRUE.

### Original Query

```sql
SELECT * FROM products
WHERE category='Accessories'
AND released=1;
```

### Modified Query

```sql
SELECT * FROM products
WHERE category='' OR 1=1--'
AND released=1;
```

### Explanation

- `'` closes the original SQL string.
- `OR 1=1` is always TRUE.
- `--` comments out the remaining part of the SQL statement.

Because the condition is always TRUE, the application returns every product, including hidden products.

---

# Result

✅ Successfully retrieved hidden products.

The application displayed products that should not normally be visible, confirming the SQL Injection vulnerability.

---

# Impact

If this vulnerability exists in a real-world application, an attacker may be able to:

- Retrieve hidden or confidential records.
- Bypass application restrictions.
- Extract sensitive database information.
- Escalate the attack to more advanced SQL Injection techniques.

---

# Mitigation

To prevent this vulnerability:

- Use Prepared Statements (Parameterized Queries).
- Validate and sanitize user input.
- Avoid dynamic SQL query construction.
- Apply the Principle of Least Privilege for database accounts.

---

# Screenshots

|         Step             |   Screenshot  |
|--------------------------|---------------|
| Original Request         | `images/original-page.png` |
| SQL Injection Test (`'`) | `images/single-quote-test.png` |
| Payload (`' OR 1=1--`)   | `images/payload.png` |
| Lab Solved               | `images/lab-solved.png` |

---

# Key Learnings

- Learned how to identify SQL Injection manually.
- Understood Boolean-based SQL Injection.
- Learned how SQL comments (`--`) modify query execution.
- Confirmed the importance of manual testing before using automation tools.

---

# Next Step

Validate this vulnerability using **SQLMap** and compare manual testing with automated testing.
