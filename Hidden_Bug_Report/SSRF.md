### 🐞 Bug Report: Server-Side Request Forgery (SSRF)

**Reported By:** Raghav Arora  
**LinkedIn:** [https://www.linkedin.com/in/iamraghavarora](https://www.linkedin.com/in/iamraghavarora)  
**Severity:** High  
**Date Reported:** 2025-06-16

---

### 📄 Summary

A **Server-Side Request Forgery (SSRF)** vulnerability was discovered in the application that allows attackers to make the server perform unauthorized or unintended requests to internal or external systems. This can potentially expose sensitive information, such as internal services, metadata endpoints, and internal network architecture.

---

### 📌 Affected Component

- Endpoint: `/fetch?url=`
- Functionality: URL Fetching / Preview / Webhooks / Image Rendering

---

### 🚨 Proof of Concept (PoC)

**Steps to Reproduce:**

1. Open a terminal and run the following `curl` command:
   ```bash
   curl "https://example-vulnerable.com/fetch?url=http://127.0.0.1:80"
   ```

2. Observe if the response contains internal server content such as:
   ```
   Welcome to nginx!
   ```

3. Further test cloud infrastructure access (e.g., AWS metadata):
   ```bash
   curl "https://example-vulnerable.com/fetch?url=http://169.254.169.254/latest/meta-data/"
   ```

4. If the above returns valid content, SSRF is confirmed.

---

### ❗ Observed Result

- The server fetches and returns data from internal/private IP addresses.
- Responses include valid content from localhost or metadata services.

---

### ✅ Expected Result

- The server should **block all requests** to private IP addresses, internal services, and cloud metadata endpoints.
- Only approved public domains should be accessible via this functionality.

Example of a safe response:
```json
{
  "error": "Invalid URL or access to this resource is not permitted."
}
```

---

### 🎯 Impact

- Access to internal systems and endpoints not exposed to the public.
- Enumeration of internal network structure and services.
- Extraction of cloud infrastructure metadata (e.g., AWS credentials).
- Possible pivot point for further internal attacks or RCE.

---

### 🛠️ Suggested Remediation

- Disallow access to private IP address ranges like:
  - `127.0.0.1/8`, `169.254.0.0/16`, `10.0.0.0/8`, `192.168.0.0/16`, etc.
- Use allowlisting for approved domains.
- Use DNS resolution checks to prevent redirection-based SSRF.
- Sanitize and validate user input URLs.
- Disable unnecessary URL-fetching functionality if not needed.

---

### 🔒 Security Best Practices

- Treat all user input as untrusted and validate rigorously.
- Block connections to internal and non-routable IPs.
- Implement logging and alerting for unusual outbound requests.
- Enforce outbound firewall rules to restrict request destinations.

---

### 🙏 Note to Security Team / Senior Reviewer

This SSRF vulnerability poses a **high risk**, especially in cloud environments where metadata endpoints can expose secrets. Immediate mitigation is recommended, followed by a full audit of all URL-fetching logic across the application.
