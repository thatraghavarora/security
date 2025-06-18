### 🐞 Bug Report: WordPress Admin Panel Exposure

**Reported By:** Raghav Arora  
**LinkedIn:** [https://www.linkedin.com/in/iamraghavarora](https://www.linkedin.com/in/iamraghavarora)  
**Severity:** Very Low  
**Date Reported:** 2025-06-16

---

### 📄 Summary

The default WordPress admin panel is accessible at `/wp-admin` and `/wp-login.php`. Exposing these predictable URLs publicly makes the site vulnerable to:

- **Brute-force login attempts**
- **Automated bot scans**
- **Credential stuffing attacks**

By default, these admin paths are not protected or hidden, making it easier for attackers to target the authentication system.

---

### 📌 Affected Component

- URLs: `/wp-admin`, `/wp-login.php`
- WordPress core admin login and dashboard access

---

### 🚨 Proof of Concept (PoC)

**Steps to Reproduce:**

1. Open any browser or use curl to access the following URLs:
   ```bash
   curl -I https://<target-site>/wp-login.php
   curl -I https://<target-site>/wp-admin
   ```

2. If accessible, you’ll receive an HTTP 200/302 response and a login page.

3. This confirms that the WordPress backend is publicly exposed.

---

### 🎯 Impact

- **Brute-force vulnerability**: Bots can try guessing credentials due to known URL.
- **Enumeration risk**: Usernames can be enumerated via login errors.
- **DDoS vector**: Automated traffic on login endpoint can consume server resources.
- **Reconnaissance aid**: Confirms CMS type, version, and layout.

---

### 🛠️ Suggested Remediation

- **Hide Admin URLs using Security Plugins**:
  - Use plugins like:
    - 🛡️ *WPS Hide Login*
    - 🛡️ *iThemes Security*
    - 🛡️ *WP Cerber Security*

  These plugins allow you to change the default login URL (e.g., from `/wp-login.php` to `/secret-login`).

- **Add Firewall Rules**:
  - Limit access to the admin panel by IP using `.htaccess` or hosting firewall.
  
    ```apache
    <Files wp-login.php>
      Order Deny,Allow
      Deny from all
      Allow from 192.168.1.100
    </Files>
    ```

- **Enable CAPTCHA and Two-Factor Authentication (2FA)**:
  - Prevent automated login attempts.

---

### 🔒 Security Best Practices

- **Do not leave login endpoints exposed** to all users.
- **Use 2FA** and CAPTCHA on login forms.
- **Rename default login paths** to add obscurity and increase attack complexity.
- **Monitor failed login attempts** and set rate limits.

---

### 🙏 Note to Security Team / Developers

The admin panel is the gateway to the entire WordPress CMS. Leaving it publicly accessible via known URLs is a security risk, especially on high-traffic or sensitive websites. Hiding and protecting it is a quick, effective way to reduce attack surface.
