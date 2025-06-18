### 🐞 Bug Report: User Enumeration via Exposed WP REST API Endpoint

**Reported By:** Raghav Arora  
**LinkedIn:** [https://www.linkedin.com/in/iamraghavarora](https://www.linkedin.com/in/iamraghavarora)  
**Severity:** Medium  
**Date Reported:** 2025-06-16

---

### 📄 Summary

An endpoint of the WordPress REST API (`/wp-json/wp/v2/users`) is publicly accessible and discloses sensitive user information such as usernames, display names, and user IDs. This can be exploited for **user enumeration attacks**, **brute-force login attempts**, or **phishing campaigns**.

---

### 📌 Affected Component

- WordPress REST API
- Endpoint: `/wp-json/wp/v2/users`

---

### 🚨 Proof of Concept (PoC)

**Steps to Reproduce:**

1. Open a browser or use curl:

   ```bash
   curl https://<target-site>/wp-json/wp/v2/users
   ```

2. The response will include a list of registered users in JSON format:

```json
[
  {
    "id": 1,
    "name": "admin",
    "slug": "admin",
    "link": "https://example.com/author/admin/",
    ...
  },
  ...
]
```

**Observed Result:**

- The endpoint discloses valid usernames and author slugs publicly.

**Expected Result:**

- The `/users` endpoint should be restricted or anonymized to prevent enumeration.

---

### 🎯 Impact

- **Usernames exposure** can be used for credential stuffing or brute-force attacks.
- **Phishing and impersonation** due to exposure of real names or display names.
- Reduces the effectiveness of usernames as secret credentials.

---

### 🛠️ Suggested Remediation

- **Disable the REST API** entirely if not needed using plugins like `Disable WP REST API`.
- **Restrict access** to `/wp-json/wp/v2/users` via authentication or permission checks.
- **Use security plugins** such as Wordfence or iThemes Security to hide user details.
- Implement **rate limiting** to reduce brute-force effectiveness.

---

### 🔒 Security Best Practices

- Always audit your WordPress API endpoints and responses.
- Do not expose usernames or IDs in public-facing endpoints.
- Rotate and harden admin credentials if enumeration is detected.

---

### 🙏 Note to Security Team / Developers

While this is a default WordPress behavior, it introduces unnecessary exposure of sensitive data. In production environments, user enumeration should always be prevented or at least logged and monitored for abuse patterns.
