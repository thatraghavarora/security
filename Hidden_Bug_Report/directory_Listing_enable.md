### 🐞 Bug Report: Directory Listing Enabled on Web Server

**Reported By:** Raghav Arora  
**LinkedIn:** [https://www.linkedin.com/in/iamraghavarora](https://www.linkedin.com/in/iamraghavarora)  
**Severity:** Medium  
**Date Reported:** 2025-06-16

---

### 📄 Summary

The web server is configured to **allow directory listing**, which means that when a user accesses a directory path that lacks a default index file (like `index.html` or `index.php`), the server responds by showing the **full list of files** in that directory. This could lead to **unintended information disclosure**.

---

### 📌 Affected Component

- Web server directory with no index file (e.g., `/uploads/`, `/backup/`, `/logs/`)
- Web server configurations (e.g., Apache, Nginx)

---

### 🚨 Proof of Concept (PoC)

**Steps to Reproduce:**

1. Navigate to a directory URL like:

```
https://example.com/uploads/
```

2. If directory listing is enabled, the server returns a page similar to:

```
Index of /uploads
------------------------
file1.jpg
file2.png
backup.zip
...
```

3. Files such as `config.bak`, `.env`, or ZIPs may be exposed unintentionally.

---

### ✅ Expected Result

The server should return a `403 Forbidden` or redirect to a safe page. Directory listing should be disabled, and no file contents should be exposed unless intended.

---

### 🎯 Impact

- **Information Disclosure**: Access to sensitive files or internal application logic.
- **Reconnaissance Vector**: Attackers gain insights into folder structure and naming conventions.
- **Data Exposure**: Unintended access to database backups, logs, or other confidential data.
- **RCE Setup**: Access to script files that may allow for remote code execution if exploitable.

---

### 🛠️ Suggested Remediation

- Disable directory listing in web server configuration:

**Apache Example (httpd.conf or .htaccess):**
```apache
Options -Indexes
```

**Nginx Example:**
```nginx
autoindex off;
```

- Ensure every public directory includes a safe `index.html` to prevent listings.
- Remove unnecessary files from web-accessible paths.
- Perform periodic audits of server folders exposed to the web.

---

### 🔒 Security Best Practices

- Principle of least privilege: Only expose what is absolutely necessary.
- Never leave sensitive files in public directories.
- Always test production servers with automated scanners or tools like Nikto, Nmap, or OWASP ZAP to detect such misconfigurations.

---

### 🙏 Note to Security Team / Senior Reviewer

Directory listing is a commonly overlooked misconfiguration that can significantly aid attackers during the reconnaissance phase. It's critical to verify that all directories are appropriately protected and do not expose file contents to unauthenticated users.
