### 🐞 Bug Report: OTP Email Bombing Vulnerability

**Reported By:** Raghav Arora  
**LinkedIn:** [https://www.linkedin.com/in/iamraghavarora](https://www.linkedin.com/in/iamraghavarora)  
**Severity:** Medium  
**Date Reported:** 2025-06-16

---

### 📄 Summary

The OTP delivery mechanism via email is vulnerable to **email bombing**, where an attacker can send a large volume of OTP emails to a victim by repeatedly invoking the OTP generation endpoint. This leads to **inbox flooding**, **denial of service**, and **user disruption**, especially when no **rate-limiting or CAPTCHA** is applied.

---

### 📌 Affected Component

- OTP generation via email (e.g., `/send-email-otp`, `/generate-otp`)
- Login, registration, or password reset workflows

---

### 🚨 Proof of Concept (PoC)

**Steps to Reproduce:**

1. Identify the endpoint that sends OTPs via email:
   ```http
   POST /generate-otp HTTP/1.1
   Host: vulnerable-site.com
   Content-Type: application/json

   {
     "email": "victim@example.com"
   }
   ```
2. Use an automated script, loop, or tool (e.g., curl, Burp Intruder) to send this request multiple times in a short period.

3. Observe that the target email receives **a flood of OTP emails**.

**Observed Result:**

- No rate-limiting or challenge mechanism in place.
- Victim receives hundreds of emails within minutes.

**Expected Result:**

- Server should restrict OTP sending frequency per email/IP/session.
- Responses should be rate-limited or trigger CAPTCHA after a few attempts.

---

### 🎯 Impact

- **Email inbox flooding** (causes important emails to be lost or ignored).
- **Resource abuse** of the backend email infrastructure (SMTP, API quota).
- **Negative user experience**; can be seen as harassment or spam.
- **Possible denial-of-service** by exhausting the victim’s inbox or storage quota.

---

### 🛠️ Suggested Remediation

- Implement **rate-limiting** (e.g., max 3 OTPs per 10 minutes per email).
- Add **cooldown periods** and **CAPTCHA** before OTP re-request.
- Use a **generic success message** to avoid enumeration or feedback loops.
- Log suspicious activity and trigger alerts for unusual OTP volumes.

---

### 🔒 Security Best Practices

- Do not allow OTPs to be sent endlessly without control.
- Monitor backend email activity per user/IP and implement abuse thresholds.
- Add email reputation monitoring to avoid blacklisting due to spam behavior.

---

### 🙏 Note to Security Team / Senior Reviewer

Though this bug doesn't allow unauthorized access, it creates **operational risk** and **user annoyance**, which could lead to account abandonment or reputation damage. Controlling outbound OTP emails is essential to maintaining system trust and functionality.
