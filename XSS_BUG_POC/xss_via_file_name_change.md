### 🐞 Bug Report: Stored XSS via File Name

**Reported By:** Raghav Arora  
**LinkedIn:** [https://www.linkedin.com/in/iamraghavarora](https://www.linkedin.com/in/iamraghavarora)  
**Severity:** High  
**Date Reported:** 2025-06-16

---

### 📄 Summary

A **Stored Cross-Site Scripting (XSS)** vulnerability was discovered where a malicious script can be injected through a **file name** during file upload. The unsanitized file name is later rendered on the page, leading to JavaScript execution in the browser context of any user viewing the file.

---

### 📌 Affected Component

- **File Upload Feature**
- **File Listing/Preview Page** that reflects uploaded file names without proper escaping.

---

### 🚨 Proof of Concept (PoC)

**Steps to Reproduce:**

1. Rename any file to the following payload:
   ```
   <script>alert('XSS via File Name')</script>.jpg
   ```

2. Upload the renamed file via the file upload form.
3. Visit the page where uploaded file names are listed or displayed.

**Observed Result:**

- The injected script runs in the browser.
- In this case, an alert box appears: `XSS via File Name`.

**Expected Result:**

- The file name should be **escaped** and displayed as plain text.
- No script execution should occur.

---

### 🎯 Impact

- Stored XSS can be triggered for **any user** viewing the page.
- Can lead to:
  - **Session hijacking**
  - **Credential theft**
  - **Phishing attacks**
  - **Admin panel takeover**
- High impact, especially if triggered in administrative or trusted contexts.

---

### 🛠️ Suggested Remediation

- Sanitize and encode file names before rendering in the UI.
- Escape characters like `<`, `>`, `"`, and `'` before output.
- Disallow special characters in file names during upload.
- Use libraries such as:
  - `DOMPurify` (Frontend)
  - `htmlspecialchars()` (PHP)
  - Built-in sanitizers in Django/Flask/Express

---

### 🔒 Security Best Practices

- Store original file names securely but use **sanitized aliases** for display.
- Never trust any part of user input, including file metadata.
- Set strong **Content Security Policy (CSP)** headers.
- Review all file-related functionalities for potential XSS vectors.

---

### 🙏 Note to Security Team / Developer

This is a **high-severity stored XSS** vulnerability that can affect all users interacting with the uploaded content. Immediate attention and remediation are recommended to prevent exploitation in production environments.
