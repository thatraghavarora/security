### 🐞 Bug Report: No Rate Limiting on Login Endpoint

**Reported By:** Raghav Arora  
**LinkedIn:** [https://www.linkedin.com/in/iamraghavarora](https://www.linkedin.com/in/iamraghavarora)  
**Severity:** High  
**Date Reported:** 2025-06-16

---

### 📄 Summary

The login endpoint does not enforce **rate limiting**, allowing an attacker to send **unlimited login attempts** without delay. This exposes the application to **brute force attacks** and **credential stuffing**, increasing the risk of unauthorized account access.

---

### 📌 Affected Component

- **Authentication Endpoint**  
  Example: `POST /login` or `/api/authenticate`

---

### 🚨 Proof of Concept (PoC)

**Steps to Reproduce:**

1. Identify the login endpoint.
2. Use a tool like **Burp Suite Intruder**, **Hydra**, or a custom script.
3. Send hundreds/thousands of login attempts with varying credentials.

```bash
for i in {1..1000}; do
  curl -X POST https://targetsite.com/login \
       -d "username=target@example.com&password=guess$i"
done
```

**Observed Result:**
- All requests receive normal server responses.
- No CAPTCHA, IP block, or cooldown is triggered.
- No alert or account lockout after repeated failed attempts.

**Expected Result:**
- Server should block or rate limit requests after 3–5 failed attempts per IP/account.
- CAPTCHA or 2FA challenge should be enforced.

---

### 🎯 Impact

- Enables **brute-force** attacks to guess weak passwords.
- Facilitates **credential stuffing** using leaked credentials from other breaches.
- May lead to **unauthorized access**, data theft, or account compromise.
- Weakens the overall **authentication security posture** of the application.

---

### 🛠️ Suggested Remediation

- Implement **rate limiting** (e.g., max 5 login attempts per minute per IP/user).
- Introduce **CAPTCHA** after a few failed attempts.
- Apply **temporary account lockout** after repeated failures.
- Log and alert on suspicious login behavior.

---

### 🔒 Security Best Practices

- Use services like **fail2ban** or **Web Application Firewalls (WAFs)** to detect and block brute-force patterns.
- Monitor authentication endpoints with a **SIEM** for unusual login attempts.
- Encourage **strong password policies** and support **2FA** for all users.

---

### 🙏 Note to Security Team / Senior Reviewer

Lack of rate limiting is a critical flaw in authentication mechanisms. Attackers regularly exploit such issues to gain unauthorized access. Proper controls must be enforced to reduce attack surface and ensure account security.
