### 🐞 Bug Report: Broken Link Hijacking Vulnerability

**Reported By:** Raghav Arora  
**LinkedIn:** [https://www.linkedin.com/in/iamraghavarora](https://www.linkedin.com/in/iamraghavarora)  
**Severity:** Medium to High  
**Date Reported:** 2025-06-16

---

### 📄 Summary

The application references external URLs or subdomains that are **no longer registered or controlled**. This opens the door to a **Broken Link Hijacking** vulnerability, where an attacker can **register the broken domain** and **inject malicious content**, perform phishing, or escalate attacks via trusted contexts (e.g., script source, iframe, image, or CNAME records).

---

### 📌 Affected Component

- External links in `href`, `img src`, `script src`, or third-party integrations
- Unused or expired **subdomains** and DNS records (e.g., CNAME still points to a decommissioned GitHub Pages or Heroku app)

---

### 🚨 Proof of Concept (PoC)

**Steps to Identify:**

1. Browse the source code or analyze network requests.
2. Locate broken links or subdomains like:

   ```
   https://example-deprecated.s3.amazonaws.com/
   https://old.examplecdn.net/
   https://projectname.github.io/
   ```

3. Perform a **WHOIS** or **DNS check** to confirm the link is unclaimed.
4. Attempt to register the domain or subdomain (if possible).
5. Host malicious payload or a spoofed version of the site.

**Observed Result:**

- The application **includes broken resources** from a third-party link that can be hijacked.
- Once hijacked, an attacker can serve malicious JavaScript, phishing pages, or tracking content.

---

### ✅ Expected Result

- The application should **not reference expired or broken links**.
- All third-party domains or subdomains should be **actively maintained** and secured.
- Decommissioned services should be fully unlinked or DNS records removed.

---

### 🎯 Impact

- **Phishing Attacks**: Users may trust the hijacked domain due to association with the primary brand.
- **Malware Injection**: If hijacked script tags or assets are loaded, attackers can perform XSS or data theft.
- **Reputation Damage**: If attackers use hijacked domains for scam or fraud.
- **Subdomain Takeover**: Especially dangerous if internal subdomains are involved (e.g., `help.example.com`, `cdn.example.com`).

---

### 🛠️ Suggested Remediation

- Regularly audit all external references and links in your HTML, JavaScript, and DNS records.
- Use tools like:
  - [Subjack](https://github.com/haccer/subjack)
  - [Amass](https://github.com/owasp-amass/amass)
  - [SecurityTrails](https://securitytrails.com/)
- Remove or update broken third-party links or domains.
- Monitor DNS and third-party dependencies continuously.
- Enable ownership validation (e.g., CNAME verification) where supported.

---

### 🔒 Security Best Practices

- Don’t reference domains or services you no longer control.
- Automate broken link checks in CI/CD pipelines.
- Use signed scripts or Subresource Integrity (SRI) where applicable.

---

### 🙏 Note to Security Team / Senior Reviewer

Broken link hijacking is an often underestimated risk that can be escalated to phishing, content injection, or brand abuse. Especially in corporate environments with many integrations and legacy systems, it is critical to maintain hygiene over third-party resources and subdomain management.
