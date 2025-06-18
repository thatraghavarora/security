### 🐞 Bug Report: OTP Verification Bypass via Country Code Manipulation

**Reported By:** Raghav Arora  
**LinkedIn:** [https://www.linkedin.com/in/iamraghavarora](https://www.linkedin.com/in/iamraghavarora)  
**Severity:** High  
**Date Reported:** 2025-06-16  

---

### 📄 Summary

An **authentication bypass** was discovered in the OTP verification system by manipulating the **country code parameter** during the phone number submission. Some applications restrict OTP delivery to certain countries (e.g., only `+91` for India), but the client-side country code can be modified, allowing unauthorized OTP delivery to any global number.

---

### 📌 Affected Component

- OTP-based authentication system  
- Phone number registration or login  
- Client-side country code handling

---

### 🚨 Proof of Concept (PoC)

**Steps to Reproduce:**

1. Open the login or signup page with phone-based OTP.
2. Intercept the request using a proxy tool (like Burp Suite or browser DevTools).
3. Identify the request that sends the phone number for OTP delivery.
4. Change the `country_code` or `phone` parameter in the request:
   ```json
   {
     "country_code": "+1",   // Changed from +91
     "phone": "5551234567"
   }
   ```
5. Submit the request.
6. OTP is successfully sent to a **non-authorized country code**.

**Observed Result:**
- OTP is delivered to a phone number in a country that should be restricted.
- Application does not validate or block unsupported country codes.

**Expected Result:**
- Application should **enforce allowed country codes server-side**, regardless of client input.

---

### 🎯 Impact

- Allows attackers to register/login from **unauthorized countries**.
- Could be used to **bypass geo-restrictions or fraud detection rules**.
- Enables **abuse of services** (spam, fake registrations) from unverified regions.
- Weakens **regional restrictions** and **telecom security models**.

---

### 🛠️ Suggested Remediation

- Enforce **server-side validation** of allowed country codes.
- Do not rely on client-side dropdowns or JavaScript for country enforcement.
- If the service is regional, **restrict OTP delivery** only to approved lists.
- Log and alert when abnormal country code usage is detected.

---

### 🔒 Security Best Practices

- Always perform **input validation on the backend**.
- Treat all client-side data as untrusted.
- Use allowlists for critical parameters like `country_code`.

---

### 🙏 Note to Security Team

This vulnerability highlights a common assumption that users will only submit valid data via the UI. Relying on frontend restrictions is insecure. A proper backend validation mechanism must always be in place to prevent such abuses.
