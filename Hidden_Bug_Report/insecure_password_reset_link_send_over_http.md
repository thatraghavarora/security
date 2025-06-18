### 🐞 Bug Report: Insecure Password Reset Link Sent Over HTTP

**Reported By:** Raghav Arora  
**LinkedIn:** [https://www.linkedin.com/in/iamraghavarora](https://www.linkedin.com/in/iamraghavarora)  
**Severity:** High  
**Date Reported:** 2025-06-16

---

### 📄 Summary

The **password reset email** sent to users contains a link that uses the **`http://` protocol** instead of the **secure `https://` protocol**. This exposes the password reset token to potential interception or manipulation by attackers during transmission, especially on insecure networks (e.g., public Wi-Fi).

---

### 📌 Affected Component

- Password Reset Email Template
- Token Generation and URL Construction Logic

---

### 🚨 Proof of Concept (PoC)

**Steps to Reproduce:**

1. Initiate a password reset using a valid email.
2. Check the received email for the reset link.
3. The link appears as:

   ```
   http://example.com/reset-password?token=abcd1234
   ```

4. Upon clicking, the token is transmitted over an **unencrypted HTTP connection**.

**Expected Behavior:**

The reset link should use:

```
https://example.com/reset-password?token=abcd1234
```

---

### 🎯 Impact

- **Token Interception:** HTTP traffic can be intercepted by network attackers (MITM).
- **Account Takeover:** An attacker can use the captured token to reset the user's password.
- **Data Integrity Issues:** Possibility of content tampering by interceptors.
- **Violation of Security Standards:** Fails OWASP best practices and industry compliance standards like GDPR.

---

### 🛠️ Suggested Remediation

- Update the password reset email template to use **HTTPS links only**.
- Enforce HTTPS site-wide by implementing **HSTS (HTTP Strict Transport Security)**.
- Redirect all HTTP traffic to HTTPS on the server side.
- Review environment configurations to ensure `https://` is used in generated URLs (e.g., `BASE_URL`, reverse proxies, etc.).

---

### 🔒 Security Best Practices

- Always use **HTTPS** for transmitting sensitive information.
- Ensure that **no part of the authentication or reset flow** is accessible over HTTP.
- Use **TLS certificates** and enforce their renewal for uninterrupted HTTPS.

---

### 🙏 Note to Security Team

This vulnerability exposes users to serious risk, especially on untrusted networks. Password reset links must be sent using secure channels only. Please prioritize fixing the URL generation logic and enforcing transport-level security.
