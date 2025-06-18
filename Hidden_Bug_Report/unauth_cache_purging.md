### 🐞 Bug Report: Unauthenticated Cache Purging Vulnerability

**Reported By:** Raghav Arora  
**LinkedIn:** [https://www.linkedin.com/in/iamraghavarora](https://www.linkedin.com/in/iamraghavarora)  
**Severity:** High  
**Date Reported:** 2025-06-16

---

### 📄 Summary

An unauthenticated cache purging vulnerability was identified on a production web server. The application exposes the ability to send `PURGE` HTTP requests without requiring any authentication, which can lead to content disruption, performance degradation, and possible cache poisoning.

---

### 📌 Affected Component

- Caching layer (e.g., Varnish, Nginx, or CDN service)
- HTTP method handling for `PURGE`

---

### 🚨 Proof of Concept (PoC)

**Step 1: Check for Active Caching**

Execute this command in a terminal with `curl` installed:

```bash
curl --head https://target-site.com/
```

**Sample Output:**

```http
via: 1.1 varnish
age: 7
x-served-by: cache-node-XYZ
x-cache: HIT
x-cache-hits: 1
```

These headers confirm that the page is being cached by an intermediary service.

---

**Step 2: Attempt to Purge the Cache Without Authentication**

```bash
curl -X PURGE https://target-site.com/
```

**Sample Response:**

```json
{ "status": "ok", "id": "1237-1678993092-222436" }
```

The cache was purged successfully without requiring any authorization.

---

### ✅ Expected Result

The server should block unauthenticated purge attempts and respond with `403 Forbidden` or `401 Unauthorized`.

---

### 🎯 Impact

- **Denial of Service (DoS):** Continuous purging forces the server to rebuild content repeatedly.
- **Content Exposure:** Newly deployed or updated content could be served prematurely or maliciously.
- **Cache Poisoning:** Attacker can purge and re-cache manipulated content.
- **Performance Hit:** High server load due to unnecessary origin fetches.

---

### 🛠️ Suggested Remediation

- Restrict `PURGE` requests to trusted internal IPs.
- Require authentication (e.g., API keys or admin tokens) for purge operations.
- Audit all endpoints that handle caching headers or control caching logic.
- Log and monitor cache purging activities for anomalies.

---

### 🔒 Security Best Practices

- Use firewall rules or `VCL` logic to enforce IP-based access control for cache invalidation.
- Avoid exposing admin HTTP methods (like `PURGE`) publicly.
- Regularly review and test your CDN or reverse proxy configurations for unexpected behaviors.

**Example Varnish Configuration:**

```vcl
acl purge {
  "localhost";
  "10.0.0.0"/8;
}

sub vcl_recv {
  if (req.method == "PURGE") {
    if (!client.ip ~ purge) {
      return (synth(403, "Forbidden"));
    }
  }
}
```

---

### 🙏 Note to Security Team / Senior Reviewer

This issue allows any external actor to purge content from cache layers, which can be exploited to reduce performance or alter content visibility. Although it doesn't result in data breach directly, it weakens the integrity and availability of your web platform. It is advised to review all caching and invalidation endpoints.
