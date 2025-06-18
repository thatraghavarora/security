### 🐞 Bug Report: Stored XSS via Comments Section

**Reported By:** Raghav Arora  
**LinkedIn:** [https://www.linkedin.com/in/iamraghavarora](https://www.linkedin.com/in/iamraghavarora)  
**Severity:** High  
**Date Reported:** 2025-06-16

---

### 📄 Summary

A **Stored Cross-Site Scripting (XSS)** vulnerability was discovered in the **comments/review section** of the website. The application fails to sanitize HTML input before rendering, allowing attackers to inject malicious JavaScript into user-generated content.

---

### 📌 Affected Component

- **Comments/Review Submission**
- **Frontend display of submitted reviews or comments**

---

### 🚨 Proof of Concept (PoC)

**Steps to Reproduce:**

1. Go to a product page or post where comments can be submitted.
2. Submit a comment with the following payload:

   ```html
   <script>alert('XSS via comment')</script>
   ```

3. Reload the page or view the comment section.

**Observed Result:**

- The script is executed whenever the comment is rendered, triggering an alert box.

**Expected Result:**

- The input should be rendered as plain text, not interpreted as HTML or JavaScript.

---

### 🎯 Impact

- **Persistent XSS** affecting all users who view the comment
- Can lead to:
  - Cookie/session theft
  - Account takeover
  - Phishing attacks
  - Compromise of admin/moderator panels if they view comments

---

### 🛠️ Suggested Remediation

- Sanitize and encode user input on both submission and rendering.
- Use a well-maintained sanitization library:
  - `DOMPurify` (JavaScript)
  - `htmlspecialchars()` (PHP)
  - `bleach` (Python)

- Disallow or strictly filter `<script>`, `<iframe>`, and other dangerous tags.

---

### 🔒 Security Best Practices

- Never trust user input, even in seemingly safe fields like comments.
- Apply **Content Security Policy (CSP)** headers to reduce script execution risk.
- Validate input on server side before storage.

---

### 🙏 Note to Security Team / Developers

Stored XSS vulnerabilities are particularly dangerous as they persist in the database and can silently affect many users. Immediate mitigation is advised to prevent large-scale exploitation.
