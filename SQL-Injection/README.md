# SQL Injection

Welcome to my **SQL Injection** learning repository based on the **PortSwigger Web Security Academy**.

This repository documents my journey of learning SQL Injection through **manual testing**, **understanding the vulnerability**, and **using SQLMap** (where appropriate) to verify findings.

Each lab contains:

- Manual testing steps
- SQL Injection payloads
- Vulnerability explanation
- Screenshots
- Impact
- Mitigation
- SQLMap verification (when applicable)

---

# What is SQL Injection?

**SQL Injection (SQLi)** is a web security vulnerability that allows an attacker to interfere with the SQL queries made by an application to its database.

When an application includes user input directly in an SQL query without proper validation or parameterized queries, an attacker can inject SQL statements that change the behavior of the query.

Example:

```sql
SELECT * FROM users
WHERE username='admin'
AND password='password';
```

If the application does not properly validate input, an attacker might enter:

```text
admin'--
```

The SQL query becomes:

```sql
SELECT * FROM users
WHERE username='admin'--'
AND password='password';
```

Everything after `--` is treated as a comment, so the password check is ignored.

---

# How Does SQL Injection Occur?

SQL Injection usually occurs when user input is directly included in an SQL query without:

- Prepared Statements
- Parameterized Queries
- Proper Input Validation

Common vulnerable inputs include:

- URL parameters
- Login forms
- Search boxes
- Registration forms
- Password reset forms
- Contact forms
- Cookies
- HTTP Headers
- API parameters

---

# Types of SQL Injection

## 1. In-Band SQL Injection

The attacker receives data directly through the application's response.

### Types

- Error-Based SQL Injection
- UNION-Based SQL Injection

---

## 2. Blind SQL Injection

The application does not display database errors or query results.

The attacker determines whether the injection works by observing the application's behavior.

### Types

- Boolean-Based Blind SQL Injection
- Time-Based Blind SQL Injection

---

## 3. Out-of-Band SQL Injection

The application sends data to another server controlled by the attacker.

This technique is less common and depends on database features.

---

# Common SQL Injection Payloads

Single Quote Test

```text
'
```

Purpose

Checks whether the application improperly handles SQL syntax.

---

Always True Condition

```text
' OR 1=1--
```

Purpose

Attempts to bypass filters or retrieve additional data.

---

Login Bypass

```text
administrator'--
```

Purpose

Attempts to bypass password verification during authentication.

---

# How to Identify Possible SQL Injection Points

Look for user-controlled inputs such as:

- URL parameters
- Login forms
- Search forms
- Product filters
- User profile forms
- API requests
- Cookies
- Hidden form fields

Example:

```text
/product?id=10
```

The parameter:

```
id
```

is a possible testing point.

---

# How to Test for SQL Injection

## Step 1

Identify a user-controlled input.

Example:

```text
/product?id=10
```

---

## Step 2

Inject a single quote.

```text
'
```

Observe the response.

Possible indicators include:

- SQL syntax error
- Database error
- Different page behavior
- HTTP 500 response

---

## Step 3

Try a Boolean payload.

```text
' OR 1=1--
```

If more data is returned than expected, the parameter may be vulnerable.

---

## Step 4

Try a False condition.

```text
' AND 1=2--
```

If the page now returns no data or behaves differently, it strongly indicates SQL Injection.

---

# How to Confirm SQL Injection

A parameter is likely vulnerable if:

- SQL errors appear.
- Application behavior changes after SQL payloads.
- Boolean TRUE and FALSE payloads produce different responses.
- Hidden or additional records become visible.
- Authentication can be bypassed.

Example:

TRUE payload

```text
' OR 1=1--
```

FALSE payload

```text
' AND 1=2--
```

If the two responses differ significantly, the parameter is likely injectable.

---

# How to Gain More Confidence That a Parameter Is Not Vulnerable

You generally **cannot prove** a parameter is completely free of SQL Injection. Instead, you increase confidence by testing thoroughly.

Consider:

- Testing multiple payloads (`'`, `"`, Boolean-based, time-based, etc.).
- Testing both GET and POST parameters.
- Testing cookies and headers if they influence application behavior.
- Using automated tools like SQLMap to supplement manual testing.
- Observing that payloads do **not** cause errors, data leakage, timing differences, or logic changes.

If none of these techniques reveal a vulnerability, the parameter is **not confirmed vulnerable** based on your testing—but avoid claiming it is "100% secure."

---

# Where Can SQL Injection Be Tested?

Possible injection points include:

- GET Parameters
- POST Parameters
- Login Forms
- Search Boxes
- Registration Forms
- Contact Forms
- Password Reset Forms
- Cookies
- HTTP Headers (User-Agent, Referer, etc.)
- JSON Request Bodies
- XML Requests
- REST API Parameters
- GraphQL Inputs

Any location where user input is used in an SQL query could potentially be tested.

---

# Manual Testing vs SQLMap

| Manual Testing | SQLMap |
|---------------|--------|
| Helps understand SQL Injection | Automates testing |
| Better for learning | Faster verification |
| Requires manual payloads | Generates payloads automatically |
| Recommended for beginners | Recommended after manual testing |

---

# SQL Injection Testing Workflow

```
Identify Input
        │
        ▼
Inject '
        │
        ▼
Observe Response
        │
        ▼
Test Boolean Payloads
        │
        ▼
Confirm SQL Injection
        │
        ▼
Understand the Vulnerability
        │
        ▼
Verify with SQLMap (if appropriate)
```

---

# Lab Progress

| Lab | Description | Manual | SQLMap | Status |
|-----|-------------|:------:|:------:|:------:|
| Lab 01 | SQL Injection in WHERE Clause Allowing Retrieval of Hidden Data | ✅ | ✅ | Completed |
| Lab 02 | SQL Injection Vulnerability Allowing Login Bypass | ✅ | N/A | Completed |
| Lab 03 | *To Be Started* | ⏳ | ⏳ | Not Started |

---

# References

- PortSwigger Web Security Academy
- OWASP SQL Injection Prevention Cheat Sheet
- SQLMap Documentation
