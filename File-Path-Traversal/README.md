# Lab 01 – File Path Traversal, Simple Case

**Difficulty:** Apprentice  
**Status:** Solved  
**Platform:** PortSwigger Web Security Academy  
**Vulnerability:** File Path Traversal (Directory Traversal)

## 1. Objective

Retrieve the contents of the `/etc/passwd` file by exploiting a File Path Traversal vulnerability in the product image display functionality.

## 2. Identifying the Vulnerability

1. Opened the PortSwigger lab and clicked **Access the Lab**.
2. Opened the lab homepage.
3. Pressed `Ctrl + U` to inspect the page source.
4. Searched for `image` to identify image references.
5. Found the image endpoint:

```http
/image?filename=1.jpg
```

The `filename` parameter controls which image file the server retrieves.

## 3. Capturing the Request Using Burp Suite

1. Opened the product image in the browser.
2. Captured the image request using Burp Suite Proxy.
3. Identified the request containing the `filename` parameter.
4. Sent the request to Burp Intruder for testing.

**Original request:**

```http
GET /image?filename=1.jpg HTTP/1.1
```

## 4. Configuring Burp Intruder

1. Selected the **Sniper** attack type.
2. Set the payload position around the image filename value.
3. Configured a Simple List payload type.
4. Added the File Path Traversal payload wordlist.
5. Started the Intruder attack.

The payload wordlist and common testing methodology are documented in the parent File Path Traversal README.

## 5. Analyzing the Attack Results

After starting the attack:

1. Compared the HTTP response status codes.
2. Examined the response lengths.
3. Identified the response corresponding to `/etc/passwd`.
4. Opened the response to verify its contents.

**Successful response characteristics:**

- HTTP status: `200 OK`
- Response content type: `image/jpeg`
- Response body contained Linux account information.

The response included entries such as:

```text
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
```

This confirmed that the application returned the contents of `/etc/passwd` instead of the requested product image.

## 6. Result

The application allowed directory traversal through the `filename` parameter.

By using directory traversal sequences, it was possible to access a file outside the intended image directory.

**Result:** Successfully retrieved `/etc/passwd` and solved the PortSwigger lab.

## 7. Security Impact

An attacker could potentially:

- Read sensitive files from the server filesystem.
- Access configuration files.
- Discover system information.
- Expose sensitive application or user data.

The actual impact depends on the files accessible to the application process.

## 8. Evidence

| Evidence | Description |
|---|---|
| 01 | PortSwigger lab overview |
| 02 | Accessing the lab homepage |
| 03 | Inspecting page source using Ctrl + U |
| 04 | Identifying the image URL and filename parameter |
| 05 | Capturing the image request in Burp Suite Proxy |
| 06 | Configuring Burp Intruder and traversal payloads |
| 07 | Analyzing Intruder attack results |
| 08 | Verifying the `/etc/passwd` response |
| 09 | PortSwigger lab solved confirmation |

## 9. Key Learning

- Learned how to identify file-handling parameters from page source.
- Practiced intercepting HTTP requests using Burp Suite.
- Learned how to configure Burp Intruder with the Sniper attack type.
- Practiced testing directory traversal payloads.
- Learned to compare response status codes and lengths.
- Verified successful file retrieval through response content.

## 10. Conclusion

This lab demonstrated how improper validation of file paths can lead to File Path Traversal vulnerabilities.

By manipulating the `filename` parameter, it was possible to retrieve `/etc/passwd` from the server filesystem.

**Lab Status: Successfully Solved.**
