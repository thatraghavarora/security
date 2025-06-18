### 🐞 Bug Report: Web Cache Deception Vulnerability

**Reported By:** Raghav Arora  
**LinkedIn:** [https://www.linkedin.com/in/iamraghavarora](https://www.linkedin.com/in/iamraghavarora)  
**Severity:** High  
**Date Reported:** 2025-06-16

---

### 📄 Summary

A **Web Cache Deception** vulnerability was discovered due to improper caching of sensitive or user-specific pages. Attackers can trick the cache server into storing personalized or sensitive content under a publicly accessible URL, allowing unauthorized users to retrieve it.

---

### 📌 Affected Component

- CDN or reverse proxy (e.g., Varnish, Cloudflare, etc.)
- Server-side caching configuration
- Endpoints exposing user-specific content (e.g., `/profile`, `/dashboard`)

---

### 🚨 Proof of Concept (PoC)

**Steps to Reproduce:**

1. Login to the application as a valid user.
2. Visit a personalized page like:  
   ```
   https://target-site.com/profile
   ```
3. Modify the URL and append a fake file extension (e.g., `.css`, `.jpg`, `.html`):  
   ```
   https://target-site.com/profile.css
   ```
4. Observe that the page still loads and returns the user-specific content (e.g., username, email, etc.)
5. Now log out and visit the same modified URL from another browser or incognito mode.

**Observed Result:**

- The **user-specific content is cached** and accessible via the spoofed URL, even by unauthenticated users.

**Expected Result:**

- The application should not cache user-specific data or should return a `Cache-Control: no-store` header for such pages.

---

### 🎯 Impact

- **Data Leakage:** Sensitive user information (name, email, balance) may be exposed.
- **Session Hijacking:** In rare cases, tokens or session identifiers could be cached.
- **Loss of Privacy:** Users’ activity and data become public through cache poisoning.
- **Regulatory Risk:** Violates GDPR, HIPAA, or data protection guidelines.

---

### 🛠️ Suggested Remediation

- Do not allow caching on pages that return personalized or user-specific data.
- Add appropriate cache headers:

```http
Cache-Control: no-store, no-cache, must-revalidate
Pragma: no-cache
```

- Validate URLs and reject unexpected extensions (e.g., block `/profile.css` if `/profile` is expected).
- Use **cache keys** based on user authentication (if caching is essential).
- Monitor and restrict cache behavior at reverse proxies/CDNs.

---

### 🔒 Security Best Practices

- Never cache pages that render personalized data.
- Use strict URL validation and input sanitization.
- Configure cache rules to ignore requests with unexpected extensions or query strings.

---

### 🙏 Note to Security Team / Senior Reviewer

Web Cache Deception is a subtle but impactful vulnerability that can lead to the unintentional exposure of sensitive data through shared caches. It is highly recommended to review all caching rules, particularly for endpoints that serve authenticated or dynamic content.
