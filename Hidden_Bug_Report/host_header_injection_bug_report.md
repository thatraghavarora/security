### 🐞 Bug Report: Host Header Injection Vulnerability

**Reported By:** Raghav Arora  
**LinkedIn:** [https://www.linkedin.com/in/iamraghavarora](https://www.linkedin.com/in/iamraghavarora)  
**Severity:** High  
**Date Reported:** 2025-06-16

---

### 📄 Summary

A **Host Header Injection** vulnerability was identified in the application. The application reflects or uses the value of the `Host` header without validation, allowing attackers to manipulate it and potentially carry out **cache poisoning**, **password reset poisoning**, **redirect manipulation**, or **phishing attacks**.

---

### 📌 Affected Component

- Web server / reverse proxy / application logic that handles host headers  
- Pages that send links via email (e.g., password reset, invite links)  
- Any logic that uses the Host header to construct absolute URLs

---

### 🚨 Proof of Concept (PoC)

**Steps to Reproduce:**

1. Send a crafted HTTP request with a manipulated `Host` header:

   ```http
   GET /reset-password?token=abc123 HTTP/1.1  
   Host: attacker.com  
   ```

2. Observe if:
   - The link in the email contains `http://attacker.com/reset-password?token=abc123`
   - The site redirects based on `Host`
   - The `Host` header is reflected in the response

**Observed Result:**

- The application reflects or uses `attacker.com` in reset links, redirects, or HTML.

**Expected Result:**

- The application should ignore untrusted `Host` values and use a **predefined trusted domain** or server-side base URL.

---

### 🎯 Impact

- **Password Reset Poisoning:** Users may receive malicious links containing an attacker-controlled domain.
- **Phishing:** Attackers can use legitimate password reset tokens on their own domain.
- **Cache Poisoning:** If combined with cache mechanisms, it may poison content.
- **Bypass Logic:** Host-based routing or filtering may be bypassed.

---

### 🛠️ Suggested Remediation

- Validate the `Host` header against a **whitelist** of known values.
- Do not use the `Host` header to build absolute URLs in user-facing links. Instead:
  - Define a secure `BASE_URL` on the backend.
- If using a reverse proxy, configure it to set and lock the correct `Host` header.
- Set the `X-Forwarded-Host` and sanitize it if it's used.

---

### 🔒 Security Best Practices

- Use absolute URLs from configuration, not user-controlled headers.
- Monitor for abnormal values in the `Host` header.
- Apply WAF rules to detect and block host header manipulation.

---

### 🙏 Note to Security Team

This bug opens the door to serious attacks like phishing and token misuse. It is critical to sanitize all client-supplied headers and rely only on server-controlled configurations when generating security-sensitive links like password resets or email verification.
