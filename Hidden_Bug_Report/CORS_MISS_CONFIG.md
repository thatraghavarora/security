### 🐞 Bug Report: Misconfigured Cross-Origin Resource Sharing (CORS)

**Reported By:** Raghav Arora  
**LinkedIn:** [https://www.linkedin.com/in/iamraghavarora](https://www.linkedin.com/in/iamraghavarora)  
**Severity:** High  
**Date Reported:** 2025-06-16

---

### 📄 Summary

A **CORS misconfiguration** was identified, allowing requests from arbitrary origins. This could enable malicious websites to interact with sensitive endpoints on behalf of authenticated users, leading to potential data theft or account compromise.

---

### 📌 Affected Component

- CORS configuration on the backend/API server

---

### 🚨 Proof of Concept (PoC)

**Steps to Reproduce:**

1. Host the following HTML on an external domain (attacker's website):

   ```html
   <script>
     fetch("https://target-website.com/api/user", {
       credentials: "include"
     })
     .then(res => res.text())
     .then(data => alert(data));
   </script>
   ```

2. When a logged-in user visits this attacker page, their session cookies are sent along.

3. If the response is returned, it means **CORS allowed unauthorized origin access**.

**Observed Result:**

- The server responds with user data even though the request came from a malicious origin.

**Expected Result:**

- The server should reject cross-origin requests from unauthorized domains.

---

### 🎯 Impact

- Unauthorized access to sensitive user data via authenticated sessions
- Cross-site data exfiltration
- Account hijacking, especially if combined with CSRF

---

### 🛠️ Suggested Remediation

- Set `Access-Control-Allow-Origin` to specific trusted domains only.
- Avoid using wildcards (`*`) in `Access-Control-Allow-Origin`, especially when credentials are used.
- Do **not** include `Access-Control-Allow-Credentials: true` unless necessary and securely scoped.
- Use CORS middleware or libraries that enforce strict validation.

---

### 🔒 Security Best Practices

- Perform regular audits of CORS policy across all environments.
- Ensure origin whitelisting is dynamic and secure.
- Avoid reflecting request origin without validation.

---

### 🙏 Note to Security Team / Developers

Misconfigured CORS policies are one of the most overlooked yet dangerous vulnerabilities, especially in APIs. Even a small oversight can lead to data leakage and account compromise. Proper validation and secure default policies are critical.
