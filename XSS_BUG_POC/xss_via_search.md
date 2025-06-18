### 🐞 Bug Report: Reflected XSS via Search Input

**Reported By:** Raghav Arora  
**LinkedIn:** [https://www.linkedin.com/in/iamraghavarora](https://www.linkedin.com/in/iamraghavarora)  
**Severity:** High  
**Date Reported:** 2025-06-16

---

### 📄 Summary

A **Reflected Cross-Site Scripting (XSS)** vulnerability was identified in the **search functionality** of the application. Unsanitized input from the search query is reflected directly into the HTML response, allowing attackers to inject and execute JavaScript.

---

### 📌 Affected Component

- **Search Endpoint**  
- Web pages that reflect the search term back into the DOM without proper output encoding

---

### 🚨 Proof of Concept (PoC)

**Steps to Reproduce:**

1. Navigate to the search page.
2. Inject the following payload into the search field:

   ```html
   <script>alert('XSS')</script>
   ```

3. Submit the search.

**Observed Result:**

- The script executes in the browser and displays an alert box.

**Expected Result:**

- The input should be escaped and rendered as plain text.

---

### 🎯 Impact

- **Immediate JavaScript execution** in victim’s browser.
- Can lead to:
  - Session hijacking
  - Cookie theft
  - Phishing attacks
  - Redirection to malicious sites

---

### 🛠️ Suggested Remediation

- **Escape user input** before inserting it into the HTML response.
- Use secure server-side and frontend frameworks that automatically handle encoding.
- Recommended methods/libraries:
  - `htmlspecialchars()` in PHP
  - `escape()` in Python/Django/Flask
  - DOMPurify (for frontend rendering)

---

### 🔒 Security Best Practices

- Always validate and sanitize user input.
- Use **Content Security Policy (CSP)** headers to reduce XSS risks.
- Regularly audit reflective parameters and user-input endpoints.

---

### 🙏 Note to Security Team / Developers

This reflected XSS vulnerability is easy to exploit and could be used in phishing attacks or to compromise user accounts. It is crucial to ensure all reflected input is properly encoded and cannot be interpreted as executable code.
