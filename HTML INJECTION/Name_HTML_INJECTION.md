````markdown
### 🐞 Bug Report: HTML Injection via Name Field in Registration Form

**Reported By:** Raghav Arora  
**LinkedIn:** [https://www.linkedin.com/in/iamraghavarora](https://www.linkedin.com/in/iamraghavarora)  
**Severity:** Medium  
**Date Reported:** 2025-06-16

---

### 📄 Summary

An **HTML Injection** vulnerability was identified in the **Name field** of the registration form. This flaw allows an attacker to inject HTML tags, which are rendered unsanitized on profile or welcome pages. The application fails to encode user input before outputting it, leading to HTML manipulation and a potential base for further exploits.

---

### 📌 Affected Component

* **User Registration Form**
* Any page where the user's name is rendered after registration (e.g., Dashboard, Profile, Welcome Banner)

---

### 🚨 Proof of Concept (PoC)

**Steps to Reproduce:**

1. Go to the registration form of the website.
2. In the **Full Name** field, input the following payload:
   ```html
   <b>Raghav</b>
````

3. Submit the form and navigate to any page that displays your username.

**Observed Result:**

```html
<b>Raghav</b>
```

The above input is rendered as bold text instead of being displayed as plain text.

**Expected Result:**

The input should be escaped and rendered as:

```html
&lt;b&gt;Raghav&lt;/b&gt;
```

This would prevent HTML from being interpreted by the browser.

---

### 🎯 Impact

* Allows attackers to manipulate the appearance of content.
* Can lead to trust issues, deceptive UI, or phishing elements embedded in user-controlled areas.
* Could be a base for future **XSS escalation** if JavaScript execution becomes possible through chained vectors.
* Affects every user who views the malicious name.

---

### 🛠️ Suggested Remediation

* Always perform **output encoding** before displaying user input.
* Escape characters such as `<`, `>`, `"`, `'` using secure libraries:

  * `htmlspecialchars()` for PHP
  * `escape()` in Django templates
  * `DOMPurify` for frontend sanitization
* Validate and sanitize inputs at both client-side and server-side.

---

```
