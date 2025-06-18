### 🐞 Bug Report: No Rate Limiting or CAPTCHA on WordPress Login Page (`/wp-login.php`)

**Reported By:** Raghav Arora  
**LinkedIn:** [https://www.linkedin.com/in/iamraghavarora](https://www.linkedin.com/in/iamraghavarora)  
**Severity:** High  
**Date Reported:** 2025-06-16

---

### 📄 Summary

The WordPress login page (`/wp-login.php`) is publicly accessible and lacks **rate limiting** and **CAPTCHA mechanisms**, exposing the site to **brute-force login attacks**. This allows an attacker to automate login attempts and potentially gain unauthorized access to admin accounts.

---

### 📌 Affected Component

- Endpoint: `/wp-login.php`
- WordPress authentication mechanism

---

### 🚨 Proof of Concept (PoC)

**Steps to Reproduce:**

1. Open the login page at:
   ```
   https://<target-site>/wp-login.php
   ```
2. Use any brute-force tool (e.g., Hydra, Burp Suite, Wfuzz) to automate multiple login attempts:
   ```bash
   hydra -l admin -P passwords.txt <target-site> http-post-form "/wp-login.php:log=^USER^&pwd=^PASS^:Invalid"
   ```
3. Observe that there are **no CAPTCHA**, **no account lockout**, and **no rate limit** in place.

---

### 🎯 Impact

- Enables **credential stuffing** and **dictionary attacks**.
- Can lead to **unauthorized admin access** if weak or reused credentials are used.
- May allow for **account enumeration** through error messages.
- Increased server load and possible **DoS (Denial of Service)** scenarios.

---

### 🛠️ Suggested Remediation

- ✅ **Implement CAPTCHA** (e.g., Google reCAPTCHA) on login form.
- ✅ **Limit login attempts** with lockouts after N failed attempts using:
  - Plugins like:
    - *Limit Login Attempts Reloaded*
    - *Wordfence Security*
    - *iThemes Security*
- ✅ **Enable Two-Factor Authentication (2FA)**.
- ✅ Add Web Application Firewall (WAF) rules to restrict login attempts.

---

### 🔒 Security Best Practices

- Enforce **strong password policies**.
- Monitor login attempts via logs and alert on brute-force patterns.
- Consider changing the login URL using plugins like *WPS Hide Login*.
- Whitelist trusted IPs for admin panel access.

---

### 🙏 Note to Security Team / Developers

Login endpoints must never be left unprotected, especially on popular CMS platforms like WordPress. Without rate limiting or CAPTCHA, the door is wide open for brute-force attacks. Addressing this with minimal effort using plugins or server-side controls will significantly improve security posture.
