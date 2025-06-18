### 🐞 Bug Report: Stored XSS via Chatbot Input

**Reported By:** Raghav Arora  
**LinkedIn:** [https://www.linkedin.com/in/iamraghavarora](https://www.linkedin.com/in/iamraghavarora)  
**Severity:** High  
**Date Reported:** 2025-06-16

---

### 📄 Summary

A **Stored Cross-Site Scripting (XSS)** vulnerability was discovered in the **chatbot component** of the application. When user input is stored and later rendered in the chatbot conversation without proper sanitization or output encoding, it results in the execution of arbitrary HTML/JavaScript code.

---

### 📌 Affected Component

- **Chatbot Input/Display Interface**
- Any endpoint where chatbot messages are rendered or replayed

---

### 🚨 Proof of Concept (PoC)

**Steps to Reproduce:**

1. Open the chatbot on the website.
2. Submit the following input in the chatbot:

   ```html
   <script>alert('XSS via Chatbot')</script>
   ```

3. Observe the response or reload the chatbot to see previous messages.

**Observed Result:**

- The script executes in the user’s browser and displays an alert box.

**Expected Result:**

- The input should be displayed as text, not interpreted or rendered as HTML/JavaScript.

---

### 🎯 Impact

- Enables attackers to inject persistent JavaScript code
- Can lead to:
  - Session hijacking
  - Credential theft
  - Phishing attacks
  - Admin panel takeover (if an admin views infected chats)

---

### 🛠️ Suggested Remediation

- Sanitize all user-generated input on both input and output.
- Apply output encoding before inserting text into the DOM.
- Use secure libraries such as:
  - `DOMPurify` (for frontend rendering)
  - `htmlspecialchars()` in PHP
  - `bleach` in Python

- Enforce a strict **Content Security Policy (CSP)** to block inline scripts.

---

### 🔒 Security Best Practices

- Treat chatbot and messaging features as untrusted input channels.
- Encode dynamic content before inserting it into the UI.
- Audit and test chat features regularly for XSS vectors.

---

### 🙏 Note to Security Team / Developers

This issue shows how dynamic messaging systems can be abused for persistent XSS. Extra care must be taken in components that automatically reflect or replay user-generated input, especially in real-time environments like chatbots.
