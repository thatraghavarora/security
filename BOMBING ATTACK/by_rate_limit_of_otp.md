### 🐞 Bug Report: OTP Rate Limit Bypass via URL Obfuscation

**Reported By:** Raghav Arora  
**LinkedIn:** [https://www.linkedin.com/in/iamraghavarora](https://www.linkedin.com/in/iamraghavarora)  
**Severity:** High  
**Date Reported:** 2025-06-16

---

### 📄 Summary

A critical vulnerability was discovered that allows **bypassing OTP sending rate limits** by using **URL obfuscation techniques** (e.g., appending `/.//` to the endpoint). This bypass enables **unrestricted OTP spamming**, leading to potential **OTP bombing**, **service abuse**, or **DoS attacks** on targeted users.

---

### 📌 Affected Component

- OTP Generation / Sending Endpoint  
  *(e.g., `/api/send-otp`, `/user/send-otp`)*

---

### 🚨 Proof of Concept (PoC)

**Normal OTP Request (Rate Limited):**
```http
POST /api/send-otp HTTP/1.1
Host: vulnerable-site.com
Content-Type: application/json

{
  "phone": "+911234567890"
}
```

**Rate-Limited Response:**
```json
{
  "error": "Too many requests. Please try again later."
}
```

---

**Bypassed OTP Request (Obfuscated URL):**
```http
POST /api/send-otp/.// HTTP/1.1
Host: vulnerable-site.com
Content-Type: application/json

{
  "phone": "+911234567890"
}
```

**Bypass Result:**
- OTP is sent again **without triggering rate limiting**.
- This can be repeated by modifying the endpoint in various forms:
  - `/api/send-otp/.//`
  - `/api/send-otp/./`
  - `/api/send-otp//`

---

### ❗ Observed Result

- Rate limit protections are bypassed using **URL obfuscation**.
- Server treats obfuscated and original URLs as different, **resetting rate limits**.

---

### ✅ Expected Result

- Server should **normalize URLs** before applying rate limiting logic.
- All endpoint variations should point to a single logical route with unified rate control.

---

### 🎯 Impact

- Allows **OTP bombing** attacks (mass OTP sends to a single number).
- Enables **brute-force of OTP delivery systems**.
- Can lead to **DoS of SMS/Email services** or harassment of users.
- Increases **infrastructure costs** due to abuse of OTP functionality.

---

### 🛠️ Suggested Remediation

- Normalize URL paths before applying rate limiting.
- Use strict endpoint mapping (e.g., use route regex matchers).
- Implement backend rate limiting based on:
  - IP address
  - Phone/email target
  - Device/session ID

---

### 🔒 Security Best Practices

- Always sanitize and normalize incoming request URLs.
- Use centralized and consistent rate limiting mechanisms.
- Log and monitor unusual patterns of repeated OTP requests.

---

### 🙏 Note to Security Team / Senior Reviewer

This vulnerability leverages a common URL parsing inconsistency that results in bypassing backend protections. It's crucial to implement URL normalization and strict routing checks to prevent such abuses, especially on sensitive functionalities like OTP sending.
