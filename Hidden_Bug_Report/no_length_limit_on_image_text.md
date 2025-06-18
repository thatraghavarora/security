### 🐞 Bug Report: No Length Restriction on File Name During Upload

**Reported By:** Raghav Arora  
**LinkedIn:** [https://www.linkedin.com/in/iamraghavarora](https://www.linkedin.com/in/iamraghavarora)  
**Severity:** Medium  
**Date Reported:** 2025-06-16

---

### 📄 Summary

The file upload functionality allows files with **excessively long filenames** to be uploaded without restriction. This can cause **server crashes**, **file system errors**, or **application instability**, especially on systems with strict path or filename length limits (e.g., 255 characters for most filesystems).

---

### 📌 Affected Component

- File Upload Functionality  
- Filename Sanitization and Validation Logic  

---

### 🚨 Proof of Concept (PoC)

**Steps to Reproduce:**

1. Create a file with an excessively long name (e.g., 500+ characters):

```bash
example : 

00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000.jpg
```

2. Upload this file using the application’s file upload feature.

3. Observe the result.

**Observed Result:**

- The file is accepted without validation.
- The server may throw a 500 error, crash, or truncate the file name.
- In some environments, this could lead to **file system corruption** or **DoS**.

---

### ✅ Expected Result

- The application should **reject file names exceeding a safe length**, typically 100–150 characters.
- A proper validation message should inform the user of the limit.

---

### 🎯 Impact

- **File system errors** or truncation during storage or retrieval.
- **Server crashes** when long file names are processed or stored.
- **Denial of Service (DoS)** if an attacker uploads many such files.
- **Resource exhaustion** (e.g., file path manipulation or long log entries).

---

### 🛠️ Suggested Remediation

- Enforce a **maximum filename length** (e.g., 100 characters).
- Strip or truncate filenames that exceed this limit.
- Sanitize inputs to remove control characters or dangerous patterns.
- Implement both frontend and backend validation for filename length.

---

### 🔒 Security Best Practices

- Always validate user input on the server, including filenames.
- Limit filename lengths to comply with OS-level constraints.
- Store files using safe internal identifiers instead of user-supplied names.

---

### 🙏 Note to Security Team / Senior Reviewer

While seemingly low-risk, this vulnerability can be exploited in automated attacks that cause backend failure or storage system errors. A simple length validation would mitigate this risk efficiently and improve system stability.
