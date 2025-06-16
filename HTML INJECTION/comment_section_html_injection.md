````markdown
# 🐞 Bug Report: Stored HTML Injection in Comment Section

**Reported By:** Raghav Arora  
**LinkedIn:** [https://www.linkedin.com/in/iamraghavarora](https://www.linkedin.com/in/iamraghavarora)  
**Severity:** Medium  
**Date:** 2025-06-16  

---

## 📄 Summary

A **stored HTML injection** vulnerability was discovered in the comment section of the web application. This occurs because user input is not properly sanitized before being stored and rendered on the page.

---

## 📌 Affected Component

- Comment Section on blog/article pages  
- Any page where user-generated content is rendered without escaping

---

## 🚨 Steps to Reproduce

1. Navigate to any blog/article where comments can be posted.
2. Submit the following payload as a comment:
   ```html
   <h2 style="color: red;">Comment Injected</h2>
````

3. Refresh the page or view the comment from another user’s session.

**Observed Result:**
The injected HTML renders directly into the page layout.

**Expected Result:**
The comment should be rendered as plain text, not interpreted as HTML.

---

## 🎯 Impact

* Allows attacker to inject malicious UI elements into trusted areas.
* Can be used to:

  * Create fake login forms
  * Trick users with deceptive content (phishing)
  * Perform scriptless attacks or chain with other bugs (XSS)
* Affects all users visiting the injected comment page

---

## 🛠 Suggested Remediation

* Sanitize all user input before storing it in the database.
* Encode output when rendering comments (`<`, `>`, `"`, `'`).
* Use libraries like:

  * `DOMPurify` (JS frontend)
  * `htmlspecialchars()` (PHP)
  * `django.utils.html.escape()` (Django)
* Apply a strict Content Security Policy (CSP) to reduce risk of advanced payloads.

---

## 🔐 Security Team Note

Stored HTML injection is particularly dangerous because the payload persists across sessions and is delivered to all visitors. This opens up the possibility of broader exploitation if chained with JS or browser quirks. It is recommended to audit all content submission forms for similar flaws.

```
```
