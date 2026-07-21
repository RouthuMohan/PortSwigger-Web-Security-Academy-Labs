# Lab 02 - SQL Injection Vulnerability Allowing Login Bypass

## Lab Information

| Field | Details |
|-------|---------|
| Platform | PortSwigger Web Security Academy |
| Category | SQL Injection |
| Difficulty | Apprentice |
| Lab Name | SQL Injection Vulnerability Allowing Login Bypass |

---

## Objective

Authenticate as the `administrator` user by exploiting a SQL Injection vulnerability in the login functionality.

---

## Vulnerability Overview

The login form constructs an SQL query using user-supplied input without proper sanitization.

An attacker can inject SQL syntax into the username field to bypass authentication and gain unauthorized access.

---

## Manual Testing

### Step 1

Open the lab.

![Lab Overview](images/00-lab-overview.png)

---

### Step 2

Navigate to the login page.

![Login Page](images/02-login-page.png)

---

### Step 3

Enter the following payload into the Username field.

```text
administrator'--
```

Enter **any password value**.

Click **Log in**.

![Payload](images/03-payload-login.png)

---

### Step 4

The SQL query becomes similar to:

```sql
SELECT * FROM users
WHERE username='administrator'--'
AND password='anything'
```

The `--` starts a SQL comment, causing the password condition to be ignored.

---

### Step 5

The application authenticates the administrator account.

Lab solved successfully.

![Solved](images/04-lab-solved.png)

---

## Impact

- Authentication Bypass
- Unauthorized Account Access
- Privilege Escalation
- Full Administrative Access

---

## Mitigation

- Use parameterized queries (Prepared Statements).
- Never concatenate user input into SQL queries.
- Validate and sanitize user input.
- Apply least-privilege permissions to database accounts.

---

## Key Learnings

- SQL Injection can affect authentication mechanisms.
- SQL comments (`--`) can bypass password validation.
- Always use prepared statements to prevent SQL Injection.
