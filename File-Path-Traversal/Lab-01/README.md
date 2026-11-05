# Lab 01 – File Path Traversal, Simple Case

**Difficulty:** Apprentice  
**Status:** Solved  
**Platform:** PortSwigger Web Security Academy

## 1. Objective

Retrieve the contents of the `/etc/passwd` file by exploiting a File Path Traversal vulnerability in the image display functionality.

## 2. Identifying the Vulnerability

- Opened the lab website.
- Inspected the page source.
- Searched for image references.
- Identified the image endpoint:

```text
/image?filename=59.jpg
```

The `filename` parameter determines which image the server retrieves.

## 3. Testing with Burp Suite

- Intercepted the image request using Burp Suite Proxy.
- Sent the request to Burp Repeater.
- Modified the `filename` parameter.

**Original request:**

```http
GET /image?filename=59.jpg HTTP/2
```

**Modified request:**

```http
GET /image?filename=../../../etc/passwd HTTP/2
```

## 4. Result

The server returned the contents of `/etc/passwd` instead of the requested image.

This confirmed that the application was vulnerable to File Path Traversal.

## 5. Impact

An attacker could potentially read sensitive files from the server's filesystem if the application does not properly validate file paths.

## 6. Evidence

| Evidence | Description |
|---|---|
| 01 | PortSwigger lab overview |
| 02 | Lab homepage |
| 03 | Identifying the image filename parameter in page source |
| 04 | Original image request |
| 05 | Intercepting the request using Burp Suite |
| 06 | Traversal payload and successful response |

## 7. Key Learning

- Identified a vulnerable file-handling parameter.
- Learned how to inspect page source to discover image filenames.
- Practiced intercepting and modifying HTTP requests.
- Used directory traversal sequences to access a file outside the intended image directory.

**Lab Result:** Successfully retrieved `/etc/passwd` and solved the lab.
