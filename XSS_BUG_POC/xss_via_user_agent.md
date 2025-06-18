### 🐞 Bug Report: Stored XSS via User-Agent Header

**Reported By:** Raghav Arora  
**LinkedIn:** [https://www.linkedin.com/in/iamraghavarora](https://www.linkedin.com/in/iamraghavarora)  
**Severity:** High  
**Date Reported:** 2025-06-16

---

### 📄 Summary

A **Stored Cross-Site Scripting (XSS)** vulnerability was discovered through the **User-Agent** HTTP header. When this header is logged or displayed in an admin or analytics interface without proper output encoding, it allows attackers to inject and execute JavaScript code in the browser of anyone viewing that page.

---

### 📌 Affected Component

- **Admin Panel** / **Logs Viewer**
- Any feature displaying HTTP headers like User-Agent without sanitization.

---

### 🚨 Proof of Concept (PoC)

**Steps to Reproduce:**

1. Use a tool like `curl` or Burp Suite to send a request with a malicious User-Agent header:

   ```bash
   curl -A "<script>alert('XSS via User-Agent')</script>" https://example.com/
   ```

2. Visit the admin/logs dashboard where User-Agent data is displayed.

**Observed Result:**

- The malicious script in the User-Agent header is executed in the browser of the admin viewing the logs.

**Expected Result:**

- The User-Agent string should be escaped and displayed as text, not interpreted as executable HTML/JS.

---

### 🎯 Impact

- **Stored XSS** affecting administrators or internal users.
- Can lead to:
  - **Session hijacking**
  - **Token/cookie theft**
  - **Account takeover**
  - **Internal dashboard compromise**

---

### 🛠️ Suggested Remediation

- Always sanitize and encode data from HTTP headers before displaying.
- Escape HTML-sensitive characters (`<`, `>`, `"`, `'`) when rendering logs.
- Use secure templating engines or libraries such as:
  - `DOMPurify` (JavaScript)
  - `htmlspecialchars()` (PHP)
  - `escape()` in Django, Flask, or Node.js

---

### 🔒 Security Best Practices

- Never trust any client-supplied input, including headers.
- Apply **Content Security Policy (CSP)** to reduce risk.
- Regularly audit logging and reporting systems for reflective or stored injection vectors.

---

### 🙏 Note to Security Team / Developers

This vulnerability represents a **critical oversight in backend logging/display** mechanisms. Since the attack vector uses headers (which are easy to spoof), it may be silently triggered until an admin unknowingly activates it. Immediate sanitization is recommended.
