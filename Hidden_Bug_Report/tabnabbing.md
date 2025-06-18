### 🐞 Bug Report: Tabnabbing Vulnerability via Unsecured External Links

**Reported By:** Raghav Arora  
**LinkedIn:** [https://www.linkedin.com/in/iamraghavarora](https://www.linkedin.com/in/iamraghavarora)  
**Severity:** Medium  
**Date Reported:** 2025-06-16

---

### 📄 Summary

A **tabnabbing vulnerability** was discovered in the application due to the use of anchor (`<a>`) tags with `target="_blank"` **without** the `rel="noopener"` or `rel="noreferrer"` attribute. This allows newly opened tabs to gain control of the original page using `window.opener`, potentially redirecting users to a **phishing page** or executing unwanted actions in the background.

---

### 📌 Affected Component

- External links using `target="_blank"` in HTML without `rel="noopener"` or `rel="noreferrer"`  
- Likely present in: blog posts, comment sections, user profile pages, or footer links.

---

### 🚨 Proof of Concept (PoC)

**Example of Vulnerable Link:**

```html
<a href="https://malicious-site.com" target="_blank">Click Here</a>
```

**Attack Steps:**

1. User clicks on a link that opens in a new tab.
2. The newly opened tab (malicious site) executes the following code on console :

```javascript
window.opener.location = "https://phishing-site.com";
```

3. The original site/tab is redirected to the attacker's phishing page **without the user noticing**.

---

### ✅ Expected Result

Links that use `target="_blank"` should **also include**:

```html
rel="noopener noreferrer"
```

This prevents the new tab from gaining access to `window.opener`, blocking the possibility of tabnabbing.

---

### 🎯 Impact

- **Phishing Attacks:** Users may unknowingly be redirected to fake login pages or scams.
- **Session Hijacking:** Redirection to attacker-controlled sites could lead to sensitive data theft.
- **User Confusion:** Users may assume the original site changed and unknowingly enter sensitive credentials.

---

### 🛠️ Suggested Remediation

- Always include `rel="noopener noreferrer"` on any external link that uses `target="_blank"`:

```html
<a href="https://example.com" target="_blank" rel="noopener noreferrer">Example</a>
```

- Perform a site-wide audit of all `target="_blank"` links and update accordingly.
- Implement content sanitization if users can input their own HTML to prevent risky links.

---

### 🔒 Security Best Practices

- Avoid using `target="_blank"` unless absolutely necessary.
- Prefer opening external links in the same tab or using secure redirection methods.
- Monitor traffic sources and log referrers for external link clicks to detect abuse.

---

### 🙏 Note to Security Team / Senior Reviewer

This is a well-documented issue that is easy to miss during development. While the exploitation doesn't require user interaction beyond clicking a link, it can result in significant phishing threats. Ensuring all links are properly configured with `rel="noopener noreferrer"` will eliminate this class of vulnerability efficiently.
