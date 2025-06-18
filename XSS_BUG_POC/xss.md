### 🐞 Bug Report: Cross-Site Scripting (XSS)

**Reported By:** Raghav Arora  
**LinkedIn:** [https://www.linkedin.com/in/iamraghavarora](https://www.linkedin.com/in/iamraghavarora)  
**Severity:** High  
**Date Reported:** 2025-06-16

---

### 📄 Summary

A **Cross-Site Scripting (XSS)** vulnerability has been identified in the application. This occurs when user input is improperly sanitized before being rendered in the browser, allowing attackers to inject malicious scripts.

---

### 📌 Affected Component

- Any user input field that is reflected on the page without proper sanitization.
  - Example: search bar, comment section, profile name, feedback form, etc.

---

### 🚨 Proof of Concept (PoC)

**Steps to Reproduce:**

1. Go to the affected input field.
2. Enter the following payload:

   ```html
   <script>alert('XSS By Raghav')</script>
   ```

3. Submit the form or trigger the reflection.

**Observed Result:**
- A JavaScript alert box pops up showing: `XSS By Raghav`.

**Expected Result:**
- The input should be rendered as plain text, not executed as a script.

---

### 🎯 Impact

- **Execution of arbitrary JavaScript** in the context of the user.
- Can lead to:
  - **Session hijacking**
  - **Cookie theft**
  - **Defacement**
  - **Phishing**
  - **Privilege escalation**
- May compromise the **entire user experience** and trust.

---

### 🛠️ Suggested Remediation

- Sanitize user input using libraries like:
  - `DOMPurify` (for frontend)
  - `htmlspecialchars()` (PHP)
  - `escape()` in Python frameworks
- Use secure templating engines that automatically escape variables.
- Apply **Content Security Policy (CSP)** headers.

---

### 🔒 Security Best Practices

- Never trust user input; always **validate, sanitize, and escape**.
- Use **output encoding** to prevent script injection.
- Perform **automated XSS testing** as part of CI/CD pipeline.

---

### 🙏 Note to Security Team / Developer

This XSS vulnerability demonstrates a classic failure to sanitize user input. It is highly recommended to conduct a full audit of all input and reflection points across the application to prevent exploitation and protect user data.
