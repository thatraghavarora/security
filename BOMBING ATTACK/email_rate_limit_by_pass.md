### 🐞 Bug Report: Email Rate Limit Bypass via Capitalization Variation

**Reported By:** Raghav Arora  
**LinkedIn:** [https://www.linkedin.com/in/iamraghavarora](https://www.linkedin.com/in/iamraghavarora)  
**Severity:** Medium  
**Date Reported:** 2025-06-16

---

### 📄 Summary

The application enforces a rate limit on sending OTPs or verification emails per email address. However, the logic fails to properly normalize the email casing. An attacker can bypass the rate limit by submitting the same email address with **different letter cases**, such as `Test@Test.com`, `TEST@test.com`, etc.

---

### 📌 Affected Component

- Email-based OTP or verification system  
- Rate Limiting Logic on Email Addresses  

---

### 🚨 Proof of Concept (PoC)

**Steps to Reproduce:**

1. Submit `test@test.com` for OTP or verification.
2. Rate limit gets applied (e.g., "Try again in 60 seconds").
3. Now send another OTP using `Test@test.com` or `TEST@test.com`.
4. OTP is sent again, bypassing the rate limit.

**Observed Behavior:**

- Server treats `test@test.com`, `Test@Test.com`, and `TEST@test.com` as different users.
- New OTPs or verification emails are sent without restriction.

**Expected Behavior:**

- Email addresses should be treated **case-insensitively** (especially the local part).
- Rate limiting should apply regardless of case variation.

---

### 🎯 Impact

- Enables attackers to bypass email OTP rate limits.
- Can be used for **email flooding** or **email spam** attacks.
- Wastes system resources and could lead to **service degradation**.
- Increases risk of **brute-force attacks** on email-based flows.

---

### 🛠️ Suggested Remediation

- Normalize all email addresses by:
  - Converting the entire email to **lowercase** before storage or processing.
  - Removing extra characters if supported by the mail provider (e.g., dots in Gmail).
- Apply rate limiting **after normalization**.

Example in code:

```python
normalized_email = email.lower()
```

---

### 🔒 Security Best Practices

- Always normalize and sanitize user input before using it for identifiers.
- Ensure rate-limiting and anti-abuse protections are applied to all forms of input consistently.

---

### 🙏 Note to Security Team

This issue demonstrates how simple logic flaws can be used to bypass essential rate-limiting protections. Email normalization is a standard and critical step in preventing abuse in authentication and verification systems.
