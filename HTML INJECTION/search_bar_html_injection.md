### 🐞 Bug Report: HTML Injection in Search Functionality

**Reported By:** Raghav Arora  
**LinkedIn:** [https://www.linkedin.com/in/iamraghavarora](https://www.linkedin.com/in/iamraghavarora)  
**Severity:** Medium  
**Date Reported:** 2025-06-16

---

### 📄 Summary

An HTML Injection vulnerability was discovered in the search feature of the web application. When a user enters HTML code into the search bar, the input is reflected back on the search results page without proper sanitization or output encoding. This allows an attacker to inject and render arbitrary HTML elements within the application interface.

---

### 📌 Affected Component

- Search Bar Input Field  
- Search Results Page where the search term is reflected back to the user

---

### 🚨 Proof of Concept (PoC)

#### Steps to Reproduce

1. Visit the application’s homepage or search page.
2. In the search bar, enter the following payload:

   `<h3>Search Injected by Raghav</h3>`

3. Submit the search query.
4. Observe how the input is reflected in the search results page.

---

### 🧪 Observed Result

The input is rendered as HTML:

`<h3>Search Injected by Raghav</h3>`

This causes a new heading to appear inside the page layout.

---

### ✅ Expected Result

The search term should be displayed as:

`&lt;h3&gt;Search Injected by Raghav&lt;/h3&gt;`

The input must be escaped so that it does not affect the page’s structure.

---

### 🎯 Impact

- Allows attackers to inject arbitrary HTML into the DOM.
- May lead to:
  - Misleading or deceptive content (fake buttons, headings, forms)
  - UI manipulation (fake messages, warnings)
  - Setup for potential XSS if the site also allows JavaScript execution
  - Damaged user trust and interface integrity
  - Can be weaponized for phishing attacks or content spoofing

---

### 🛠️ Suggested Remediation

- Sanitize and escape all user-supplied data before rendering it in the DOM.
- Use server-side or client-side escaping functions to ensure that tags like `<`, `>`, `"`, and `'` are safely encoded.

**Recommended libraries/tools:**

- `htmlspecialchars()` in PHP
- `escape()` or template engine sanitizers in Django, Flask, or Node.js
- `DOMPurify` for frontend rendering safety
- Apply a strict Content Security Policy (CSP) to reduce XSS risk

---

### 🔒 Security Best Practices

- Never trust client input, even in a basic search field.
- Apply consistent sanitization across all user-controlled inputs.
- Always encode data before rendering it in the browser.

---

