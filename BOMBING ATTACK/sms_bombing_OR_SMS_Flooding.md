### 🐞 Bug Report: OTP Flooding via Repeated OTP Generation Requests

**Reported By:** Raghav Arora  
**LinkedIn:** [https://www.linkedin.com/in/iamraghavarora](https://www.linkedin.com/in/iamraghavarora)  
**Severity:** Medium  
**Date Reported:** 2025-06-16

---

### 📄 Summary

An attacker can **flood a user's device with OTPs** by repeatedly triggering the OTP generation endpoint (e.g., for login or password reset), leading to potential **user disruption**, **SMS/Email service abuse**, and **resource exhaustion**.

---

### 📌 Affected Component

- OTP Generation Endpoint (e.g., `/generate-otp`, `/send-otp`)
- Login or password recovery flows that send OTPs via SMS or email

---

### 🚨 Proof of Concept (PoC)

**Steps to Reproduce:**

1. Capture the OTP generation request:
   ```http
   POST /generate-otp HTTP/1.1
   Host: target-website.com
   Content-Type: application/json

   {
     "phone": "+91xxxxxxxxxx"
   }
   ```
2. Send this request repeatedly using a tool (Burp Intruder, curl loop, or a script).

3. The victim receives **continuous OTPs via SMS or email** without any rate limit.

**Observed Result:**

- OTP is sent each time the request is made, without any throttling.
- Victim receives dozens or hundreds of OTPs in a short time.

**Expected Result:**

- A **rate limit** or **cooldown timer** should restrict OTP generation (e.g., once per 30 seconds).
- System should return an error or CAPTCHA after several requests.

---

### 🎯 Impact

- **SMS or email flooding** of legitimate users.
- **Denial of service** or confusion during login/registration.
- **Monetary loss** if OTPs are sent via paid SMS gateways.
- **Reduced trust** due to perceived spamming or malfunction.

---

### 🛠️ Suggested Remediation

- Implement **rate limiting** on OTP generation per phone/email/IP (e.g., max 3 per 10 minutes).
- Use **temporary blocking** after multiple requests.
- Add **CAPTCHA** to OTP generation forms to prevent automated abuse.
- Return a **generic success message** without indicating whether an OTP was sent.

---

### 🔒 Security Best Practices

- Monitor OTP generation activity and alert on anomalies.
- Use token-based rate controls (e.g., IP/session/device).
- Avoid exposing internal OTP logic or identifiers in responses.

---

### 🙏 Note to Security Team / Senior Reviewer

This issue, while not allowing direct access, can be weaponized to disrupt users and abuse OTP infrastructure. Limiting OTP generation frequency is essential to maintaining service integrity and preventing user harassment.
