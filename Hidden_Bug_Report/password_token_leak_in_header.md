### 🐞 Bug Report: Password Reset Token Leak in HTTP Headers

**Reported By:** Raghav Arora  
**LinkedIn:** [https://www.linkedin.com/in/iamraghavarora](https://www.linkedin.com/in/iamraghavarora)  
**Severity:** High  
**Date Reported:** 2025-06-16

---

### 📄 Summary

A **sensitive token**, specifically a **password reset token**, was found being transmitted via **HTTP headers** during the password reset flow. This exposes the token to various attack vectors, including logging systems, browser history, and intermediate proxies, increasing the risk of **account takeover**.

---

### 📌 Affected Component

- Password Reset Functionality  
- HTTP Headers (e.g., `Authorization`, `X-Reset-Token`)

---

### 🚨 Proof of Concept (PoC)

**Steps to Reproduce:**

1. Request a password reset from the application.
2. Capture the reset request via an intercepting proxy (e.g., Burp Suite).
3. Observe the outgoing HTTP request:

   ```
   POST /api/v1/reset-password HTTP/1.1  
   Host: example.com  
   Authorization: Bearer <reset-token>  
   ```

   or

   ```
   X-Reset-Token: <token>
   ```

**Observed Result:**

- The reset token is exposed in **HTTP headers**, which can be:
  - Logged by proxies
  - Stored in browser history (if redirect used with headers)
  - Captured by any intermediate system between client and server

**Expected Result:**

- The token should be sent in the **HTTP body** or as a **one-time POST parameter**, not in headers.

---

### 🎯 Impact

- **Account Takeover (ATO)** if token is leaked or logged
- **Sensitive Information Exposure** via logs or browser dev tools
- **Violation of secure design principles** around token confidentiality
- Easier exploitation in shared environments or insecure networks

---

### 🛠️ Suggested Remediation

- Avoid placing password reset or auth tokens in HTTP headers.
- Use **POST body** to transmit such tokens securely (e.g., `application/json` or `form-data`).
- Set tokens to **expire quickly** and **invalidate after first use**.
- Use HTTPS **strictly** to prevent interception.
- Ensure **logs redact or ignore headers** containing sensitive information.

---

### 🔒 Security Best Practices

- Never transmit secrets in headers unless absolutely necessary and safe.
- Use dedicated token handling systems with encryption and short expiration times.
- Implement **audit logging** to detect token misuse or replay.

---

### 🙏 Note to Security Team

This bug introduces serious risks due to mishandling of sensitive tokens. Password reset workflows must be airtight in security and privacy. Please review all authentication flows to ensure secrets are not transmitted in insecure ways, especially via headers or query strings.
