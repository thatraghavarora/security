### 🐞 Bug Report: Origin IP Address Disclosure

**Reported By:** Raghav Arora  
**LinkedIn:** [https://www.linkedin.com/in/iamraghavarora](https://www.linkedin.com/in/iamraghavarora)  
**Severity:** Medium  
**Date Reported:** 2025-06-16

---

### 📄 Summary

The **origin IP address** of the application server has been exposed. This is a security misconfiguration that reveals the true IP address behind a CDN, WAF, or reverse proxy, which can potentially be used to **bypass security layers** and launch direct attacks against the origin server (e.g., DDoS, brute-force, or exploit attempts).

---

### 📌 Affected Component

- Network infrastructure
- CDN/WAF configuration
- DNS records or misconfigured services leaking the real IP

---

### 🚨 Proof of Concept (PoC)

**Methods used to identify the origin IP:**

- **Direct IP access**: Visiting the origin IP in a browser returns the same application content.
- **Security misconfigurations**: DNS records (A or PTR) point directly to origin server.
- **HTTP Headers**: Headers like `X-Origin-IP`, `X-Forwarded-For`, or `Via` reveal internal addresses.
- **CORS misconfiguration**: Leak via improperly configured CORS responses.
- **Third-party services**: Using tools like Shodan or Censys to match TLS certificate or favicon hashes.

**Example Discovery (Simulated):**

```bash
curl -I http://[origin-ip]
```

**Observed Result:**

The server responds with:

```
HTTP/1.1 200 OK
Server: nginx
Content-Type: text/html
```

Or displays the same content as the official website, confirming the IP belongs to the origin server.

---

### ✅ Expected Result

- The origin IP should remain **hidden behind a CDN/WAF**.
- Any direct request to the IP should return an error, timeout, or 403 Forbidden.
- All DNS records should only expose the CDN endpoint.

---

### 🎯 Impact

- **DDoS Attacks**: Attackers can bypass CDN protections and overwhelm the origin server.
- **Security Bypass**: WAF and firewall rules may be circumvented when the origin IP is known.
- **Reconnaissance**: Revealing the infrastructure allows attackers to fingerprint services or OS.
- **Data Breach Risk**: Unpatched services may be directly exploited at the origin.

---

### 🛠️ Suggested Remediation

- Restrict access to the origin server to allow requests **only from the CDN/WAF IP ranges**.
- Configure firewall rules to block public access to the origin IP.
- Remove any A records in DNS pointing directly to the origin.
- Disable response to direct IP access at the server level.
- Regularly monitor for origin exposure using tools like Shodan or Censys.

---

### 🔒 Security Best Practices

- Always shield backend infrastructure behind proxies or CDNs.
- Enforce strict firewall access rules for application layer traffic.
- Perform regular infrastructure scans to detect accidental exposure.

---

### 🙏 Note to Security Team / Senior Reviewer

While origin IP exposure may not be a vulnerability by itself, it opens the door to several high-impact attacks. It is critical to keep infrastructure details hidden from public access and enforce access control at the network layer.
