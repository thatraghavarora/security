### 🐞 Bug Report: HTML Injection in Registration Confirmation Email

**Reported By:** Raghav Arora  
**LinkedIn:** [https://www.linkedin.com/in/iamraghavarora](https://www.linkedin.com/in/iamraghavarora)  
**Severity:** Medium  
**Date Reported:** 2025-06-16

---

### 📄 Summary

An **HTML Injection** vulnerability was identified in the **registration confirmation email** sent to users. The application does not properly sanitize the `first name` field during registration, and any HTML code entered in this field is rendered directly in the confirmation email.

---

### 📌 Affected Component

- **User Registration Module**
- **Email Template Rendering for Welcome/Confirmation**

---

### 🚨 Proof of Concept (PoC)

**Steps to Reproduce:**

1. Register a new user account with the following first name:

   ```html
   <h1>Hacked by Raghav</h1>
   ```

2. Check the email received upon successful registration.

**Observed Result:**

The HTML is rendered inside the email body:

```html
<h1>Hacked by Raghav</h1>
```

This appears as a large heading in the email content.

**Expected Result:**

The email should display the raw text:

```
<h1>Hacked by Raghav</h1>
```

without interpreting it as HTML.

---

### 🎯 Impact

- **Phishing Opportunity:** Emails can include misleading headers, forms, or links.
- **Brand Trust Damage:** Injected HTML can harm branding or confuse users.
- **Potential for Script Execution:** If the email client does not sanitize properly or if combined with unsafe elements like `<img onerror>`, can lead to XSS.
- **HTML-Based Email Spoofing:** Attackers could design fake messages in legitimate emails.

---

### 🛠️ Suggested Remediation

- Sanitize user inputs (like `first name`) before including them in HTML emails.
- Use proper encoding functions:
  - `htmlspecialchars()` in PHP
  - `escape()` in Python, Node.js, or template engines
- Validate fields like names to allow only alphabetic characters or known safe patterns.

---

### 🔒 Security Best Practices

- Treat all user input as untrusted.
- Use server-side escaping before injecting dynamic data into emails.
- Apply content security policies and email security guidelines.

---

### 🙏 Note to Security Team

While this issue might seem cosmetic, it opens doors for phishing and misleading content in transactional emails. It's important to sanitize all user data reflected back in communication channels.
