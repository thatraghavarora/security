### 🐞 Bug Report: OTP Exposed in API Response

**Reported By:** Raghav Arora  
**LinkedIn:** [https://www.linkedin.com/in/iamraghavarora](https://www.linkedin.com/in/iamraghavarora)  
**Severity:** High  
**Date Reported:** 2025-06-16

---

### 📄 Summary

A critical vulnerability was discovered in the OTP delivery mechanism where the OTP is **exposed directly in the API response** after submitting a request to generate or resend the OTP. This makes it possible for an attacker to **bypass OTP verification** entirely, gaining unauthorized access without needing access to the victim's phone or email.

---

### 📌 Affected Component

- OTP Generation Endpoint (e.g., `/generate-otp`, `/send-otp`)
- Login / Signup / Forgot Password flows

---

### 🚨 Proof of Concept (PoC)

**Steps to Reproduce:**

1. Intercept or observe the request to the OTP generation endpoint:
   ```http
   POST /api/send-otp HTTP/1.1
   Host: vulnerable-website.com
   Content-Type: application/json

   {
     "phone": "+911234567890"
   }
   ```

2. Observe the **response** from the server:
   ```json
   {
     "status": "success",
     "otp": "482915",
     "message": "OTP sent successfully"
   }
   ```

3. Use the OTP directly to authenticate without ever needing access to the user’s phone.

---

### ❗ Observed Result

The OTP is returned in plaintext in the API response:
```json
"otp": "482915"
```

---

### ✅ Expected Result

The response should **not include the OTP**. OTP should only be delivered via a secure channel (SMS, email), and the API response should only confirm that the OTP has been sent.

Example of a safe response:
```json
{
  "status": "success",
  "message": "OTP has been sent to your registered mobile number."
}
```

---

### 🎯 Impact

- Complete **bypass of OTP-based authentication**.
- Allows **unauthorized access** to any account by automating OTP request and reuse.
- Can lead to **account takeover**, **data theft**, or **privilege escalation**.
- Poses a severe risk to systems relying on OTP for second-factor authentication.

---

### 🛠️ Suggested Remediation

- **Never return OTPs in client-visible API responses.**
- Deliver OTPs only via secure channels like SMS or email.
- Store OTPs securely on the server and verify server-side without exposing them.
- Add logging and alerts for suspicious OTP activity.

---

### 🔒 Security Best Practices

- Treat OTPs like passwords or tokens — never expose them in frontend responses.
- Use secure logging practices to avoid accidentally logging OTPs.
- Implement rate-limiting and expiration policies for OTPs.

---

### 🙏 Note to Security Team / Senior Reviewer

This is a **critical vulnerability** that undermines the entire OTP security mechanism. It is essential to fix this immediately to prevent automated account takeovers or targeted attacks using this exploit vector.
