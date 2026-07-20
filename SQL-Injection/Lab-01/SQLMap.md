# Lab 01 - SQLMap Verification

## Lab Information

| Field | Details |
|-------|---------|
| Tool | SQLMap |
| Platform | PortSwigger Web Security Academy |
| Lab | SQL Injection Vulnerability in WHERE Clause Allowing Retrieval of Hidden Data |
| Testing Method | Automated |
| Status | ✅ Verified |

---

# Objective

Verify the SQL Injection vulnerability discovered during manual testing using SQLMap.

This verification confirms that the `category` parameter is vulnerable without performing unnecessary database enumeration.

---

# Target

```
https://<lab-url>/filter?category=Accessories
```

Replace `<lab-url>` with your PortSwigger lab URL.

---

# SQLMap Command

```bash
sqlmap -u "https://<lab-url>/filter?category=Accessories" --batch
```

Example:

```bash
sqlmap -u "https://0ab60019045f1d3382d2331700ae0002.web-security-academy.net/filter?category=Accessories" --batch
```

---

# Command Explanation

| Option | Description |
|---------|-------------|
| `-u` | Specifies the target URL. |
| `--batch` | Runs SQLMap non-interactively using default answers. |

---

# Expected Result

SQLMap automatically tests the `category` parameter.

If vulnerable, SQLMap reports that the parameter appears injectable.

No database enumeration was performed because this lab only requires vulnerability verification.

---

# Screenshot

Add the SQLMap terminal output here.

Example:

![SQLMap Verification](images/05-sqlmap-verification.png)

---

# Conclusion

Manual testing successfully identified the SQL Injection vulnerability.

SQLMap independently verified the same vulnerability using automated testing.

Both methods confirmed that the `category` parameter is vulnerable to SQL Injection.
