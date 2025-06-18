### 🐞 Bug Report: Custom OTP Injection via Request Parameter

**Reported By:** Raghav Arora  
**LinkedIn:** [https://www.linkedin.com/in/iamraghavarora](https://www.linkedin.com/in/iamraghavarora)  
**Severity:** High  
**Date Reported:** 2025-06-16

---

### 📄 Summary

A critical vulnerability was found where the server **accepts and processes user-supplied OTP values** via request parameters during OTP generation. This allows an attacker to send **arbitrary OTPs** to a victim's email or phone, enabling **OTP prediction or forced bypass** attacks.

---

### 📌 Affected Component

- OTP Generation or Sending Endpoint  
  *(e.g., `/api/send-otp`, `/user/generate-otp`)*

---

### 🚨 Proof of Concept (PoC)

**Request Used:**
```http
POST /api/send-otp HTTP/1.1
Host: vulnerable-site.com
Content-Type: application/json

{
  "phone": "+911234567890",
  "otp": "123456"
}
```

**Observed Result:**
- The OTP `123456` is accepted and sent to the user.
- The same `123456` can be used to verify the user in the next step.

**Expected Result:**
- The server should **generate a secure, random OTP server-side**.
- The OTP value should **not be accepted from the client-side request**.

---

### 🎯 Impact

- Attackers can send predictable OTPs (e.g., `000000`, `123456`) to users.
- Bypasses the security of the OTP mechanism.
- Can be used for:
  - **Brute-force OTP verification**
  - **Account takeover**
  - **Phishing-like attacks with pre-known OTPs**
- Completely **breaks the integrity** of OTP-based authentication flows.

---

### 🛠️ Suggested Remediation

- Never accept OTP value from the request payload.
- Always generate OTPs server-side using secure random functions.
- Validate OTPs strictly with server-stored values.
- Implement rate limiting and OTP expiration policies.

---

### 🔒 Security Best Practices

- Treat OTP as a **security token** — never trust client-supplied values.
- Log and monitor abnormal behavior (e.g., fixed OTPs used across requests).
- Ensure OTP generation and validation happen only on the backend.

---

### 🙏 Note to Security Team / Senior Reviewer

This vulnerability indicates a **logic flaw** in the OTP workflow, where client control compromises the authentication mechanism. Fixing this should be prioritized to restore secure authentication practices across the platform.
