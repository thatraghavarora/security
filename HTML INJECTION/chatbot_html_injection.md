

### 🐞 Bug Report: HTML Injection in Chatbot Response

**Reported By:** Raghav Arora
**LinkedIn:** [https://www.linkedin.com/in/iamraghavarora](https://www.linkedin.com/in/iamraghavarora)
**Severity:** Medium
**Date Reported:** 2025-06-16

---

### 📄 Summary

A **reflected HTML injection** vulnerability was identified in the chatbot component of the web application. This occurs when user-supplied input is not properly sanitized before being reflected back into the DOM in chat messages.

---

### 📌 Affected Component

* **Chatbot Frontend**
* Endpoint or component where chatbot displays user messages

---

### 🚨 Proof of Concept (PoC)

**Step to Reproduce:**

1. Open the chatbot on the target web page.
2. Enter the following payload into the chatbot input:

   ```html
   <h1 style="color:red;">Hacked By Raghav</h1>
   ```
3. The chatbot reflects the message back **without encoding**, rendering the HTML.

**Observed Result:**

```html
<h1 style="color:red;">Hacked By Raghav</h1>
```

Is **executed as HTML** and displayed as a large red heading inside the chatbot window.

**Expected Result:**
The input should be **escaped** and rendered as text, not interpreted as HTML.

---

### 🎯 Impact

* Allows an attacker to **inject HTML elements** like `<iframe>`, `<img>`, or **malicious links**.
* In some cases, this can lead to **JavaScript execution (XSS)** if combined with other vectors.
* **Phishing** or **UI redress attacks** become possible if elements are styled maliciously.
* Could impact **chatbot-integrated platforms**, customer trust, or internal tools using the bot.

---

### 🛠️ Suggested Remediation

* **Sanitize all user input** before reflecting it in the chatbot.
* Escape characters like `<`, `>`, `"`, `'` using libraries such as:

  * `DOMPurify` (JavaScript)
  * `htmlspecialchars()` (PHP)
  * `bleach` (Python)
* Apply **Content Security Policy (CSP)** to mitigate the impact of HTML/JS injections.

---

### 🙏 Note to Security Team / Senior Reviewer

This issue highlights the importance of **output encoding in dynamic content systems** like chatbots. While this injection does not execute scripts directly, it lays the groundwork for more serious vulnerabilities. I recommend reviewing similar user-reflective areas across the platform.

---

Would you like a version of this in PDF, Markdown, or GitHub issue template format too?
