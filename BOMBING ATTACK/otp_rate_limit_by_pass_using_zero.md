### 🐞 Bug Report: OTP Rate Limit Bypass via Leading Zeros in Phone Number

**Reported By:** Raghav Arora  
**LinkedIn:** [https://www.linkedin.com/in/iamraghavarora](https://www.linkedin.com/in/iamraghavarora)  
**Severity:** Medium  
**Date Reported:** 2025-06-16

---

### 📄 Summary

The application implements OTP rate limiting per phone number, but this protection can be bypassed by submitting the same number with **leading zeros**. The backend treats these variations as different inputs, allowing an attacker to send multiple OTPs to the same number without triggering the rate limit.

---

### 📌 Affected Component

- OTP Generation API  
- Rate Limiting Logic on Phone Number Input  

---

### 🚨 Proof of Concept (PoC)

**Steps to Reproduce:**

1. Go to the OTP request or login page.
2. Enter the phone number normally (e.g., `9876543210`) and request OTP.
3. Try to resend the OTP — a rate limit is triggered (e.g., "Try again in 60 seconds").
4. Now re-submit the form with the same number but with **leading zero(s)** (e.g., `09876543210`, `009876543210`).
5. Observe that the system sends a new OTP each time, bypassing the original rate limit.

**Observed Behavior:**

- Rate limiting is not triggered for numbers submitted with varying leading zeros.
- The OTP is sent again to the same phone number.

**Expected Behavior:**

- The backend should **normalize the number format** (strip leading zeros, country code handling) before applying rate limits.

---

### 🎯 Impact

- Allows attackers to **bypass OTP request limits**.
- Can lead to **OTP flooding** or **SMS bombing**.
- May result in **service abuse** or increased SMS gateway costs.
- Poses risk of **DoS (Denial of Service)** to victim users.

---

### 🛠️ Suggested Remediation

- Normalize phone numbers server-side before storing or processing (e.g., remove leading zeros, apply consistent country code formatting).
- Apply rate limiting **after normalization**.
- Use libraries like [libphonenumber](https://github.com/google/libphonenumber) for consistent number parsing and validation.

---

### 🔒 Security Best Practices

- Treat phone numbers as unique identifiers only **after normalization**.
- Store hashed and normalized versions of phone numbers for rate-limiting and lookup.
- Monitor OTP request logs for suspicious patterns and flood attempts.

---

### 🙏 Note to Security Team

Phone number normalization is often overlooked in OTP systems. This bug highlights how formatting inconsistencies can lead to logical bypasses. A proper normalization layer should be enforced across all authentication and validation systems.
