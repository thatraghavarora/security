### 🐞 Bug Report: Unrestricted File Upload Vulnerability

**Reported By:** Raghav Arora  
**LinkedIn:** [https://www.linkedin.com/in/iamraghavarora](https://www.linkedin.com/in/iamraghavarora)  
**Severity:** Critical  
**Date Reported:** 2025-06-16

---

### 📄 Summary

The application allows **unrestricted file uploads** without properly validating or restricting file types. This enables attackers to upload potentially **malicious files**, including `.php`, `.js`, `.html`, `.css`, or other executable scripts that can be used to compromise the server, perform cross-site scripting (XSS), or manipulate the client interface.

---

### 📌 Affected Component

- File upload endpoint (e.g., `/upload`, `/submit`, or profile upload form)
- Any form or API that accepts and stores files on the server

---

### 🚨 Proof of Concept (PoC)

**Steps to Reproduce:**

1. Navigate to the file upload section (e.g., profile picture upload).
2. Select and upload a file with the `.php` extension containing the following payload:

```php
<?php echo "Hacked by Raghav"; ?>
```

3. Upload completes successfully and the file is accessible via a public URL like:

```
https://example.com/uploads/hack.php
```

4. Accessing the file executes the PHP code on the server.

**Observed Result:**

- The server accepts arbitrary file types, including dangerous executable scripts.
- Uploaded files are accessible and executed (in the case of PHP or HTML).

**Expected Result:**

- The application should **only allow specific file types**, such as `.jpg`, `.png`, `.pdf`.
- Executable file uploads should be **rejected or sanitized**, and **never executed**.

---

### 🎯 Impact

- **Remote Code Execution (RCE)** via uploaded `.php`, `.jsp`, or `.asp` files
- **Cross-Site Scripting (XSS)** via `.html` or `.js` files
- **Data Exfiltration** or server compromise
- **Phishing** or malware distribution through hosted malicious pages
- Full **server takeover** in some misconfigured environments

---

### 🛠️ Suggested Remediation

- **Whitelist allowed file types** (e.g., `.jpg`, `.png`, `.pdf`)
- Validate MIME types and **magic bytes**, not just file extensions
- Rename uploaded files to random names and store them outside web root
- Apply server-side restrictions (e.g., `.htaccess` in Apache to block execution)
- Use content scanning and antivirus solutions for uploaded files

Example `.htaccess` to prevent execution:

```apache
<FilesMatch "\.(php|js|html|htm|exe|sh)$">
    Order Allow,Deny
    Deny from all
</FilesMatch>
```

---

### 🔒 Security Best Practices

- Never trust file extensions alone — always validate content type.
- Disable direct access to uploaded files when possible.
- Set strict permissions (e.g., 0644) and store files outside public directories.
- Monitor uploaded content and storage paths for suspicious activity.

---

### 🙏 Note to Security Team / Senior Reviewer

This vulnerability presents a severe risk to the entire infrastructure. Immediate patching is necessary to prevent exploitation by attackers uploading web shells, executing scripts, or delivering phishing content through trusted domains.
