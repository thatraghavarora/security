### 🐞 Bug Report: HTML Injection in Product Review Section

**Reported By:** Raghav Arora  
**LinkedIn:** [https://www.linkedin.com/in/iamraghavarora](https://www.linkedin.com/in/iamraghavarora)  
**Severity:** Medium  
**Date Reported:** 2025-06-16

---

### 📄 Summary

An HTML Injection vulnerability was discovered in the product review section of the e-commerce platform. When submitting a product review, user input is rendered directly into the product detail page without proper sanitization. This allows attackers to inject HTML elements that alter the UI or mislead users.

---

### 📌 Affected Component

- Product Review Input Field  
- Product Detail Page (where reviews are displayed)

---

### 🚨 Proof of Concept (PoC)

**Steps to Reproduce:**

1. Navigate to any product page.
2. Submit a review using the following payload in the review text box:

   `<h2 style="color:red;">Fake Review Injected by Raghav</h2>`

3. Submit the review and return to the product detail page.

---

### 🧪 Observed Result

The following input is rendered directly on the page:

```html
<h2 style="color:red;">Fake Review Injected by Raghav</h2>
```

This creates a large red heading within the review section, making it look like an official notice or UI element.

---

### ✅ Expected Result

The HTML should be escaped and rendered as plain text:

```
<h2 style="color:red;">Fake Review Injected by Raghav</h2>
```

---

### 🎯 Impact

- Allows attackers to modify the appearance of the product page.
- May mislead users with fake UI elements (e.g., warnings, buttons, announcements).
- Can be used for:
  - Phishing (fake "Buy Now" buttons)
  - Fake endorsements or reviews
  - UI disruption
- Increases risk of XSS if JS injection is possible through advanced payloads.

---

### 🛠️ Suggested Remediation

- Sanitize and escape all user-generated content before rendering it into the DOM.
- Ensure proper output encoding on the server side or before rendering on the frontend.

**Recommended Fixes:**
- Use `htmlspecialchars()` in PHP
- Use escaping filters in Django/Flask templates
- Use `DOMPurify` for frontend rendering if needed
- Apply strict Content Security Policy (CSP)

---

### 🔒 Security Best Practices

- Always escape output from user inputs, especially in dynamic sections like reviews.
- Perform input validation to block malicious content before saving it.
- Use a templating engine that automatically escapes variables by default.

---


