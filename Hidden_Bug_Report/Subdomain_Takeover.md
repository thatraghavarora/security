# 🐞 Subdomain Takeover via GitHub Pages

---

## 📌 Title

**Subdomain Takeover of `subdomain.vulnerable-website.com` due to Unused GitHub Pages Deployment**

---

## 📝 Description

A **Subdomain Takeover** vulnerability exists on `subdomain.vulnerable-website.com`, which points to a GitHub Pages domain (`username.github.io`) via a CNAME record, but no GitHub repository exists to serve content.

This allows a malicious actor to claim the GitHub username or repository and host arbitrary content at the subdomain, potentially leading to phishing, session hijacking, defacement, or malware distribution.

---

## 🌐 Affected Subdomain

- **Subdomain**: `subdomain.vulnerable-website.com`
- **CNAME Record**: `username.github.io.`

---

## 🧪 Steps to Reproduce

### ✅ Prerequisites

- GitHub account
- Domain name resolution tool (dig, nslookup)
- Web browser

---

### 🔁 Step-by-Step

#### Step 1: Identify dangling CNAME

Run:

```bash
dig CNAME subdomain.vulnerable-website.com
```

✅ Output:

```
subdomain.vulnerable-website.com.  300 IN CNAME username.github.io.
```

#### Step 2: Visit the subdomain

Navigate to:

```
https://subdomain.vulnerable-website.com
```

✅ You'll see:

```
404 There isn't a GitHub Pages site here.
```

This confirms the subdomain is pointing to a **non-existent GitHub Pages site**.

---

#### Step 3: Register/Claim the repository

If the GitHub username `username` is available:
- Register the GitHub account: `https://github.com/username`

If the username is taken:
- Create a repository called: `username.github.io`

Push an `index.html` file with test content:

```html
<h1>Subdomain Takeover Test</h1>
```

Enable GitHub Pages under repository settings.

---

#### Step 4: Visit Subdomain Again

Visit `https://subdomain.vulnerable-website.com`

✅ Your test page is now live under the target's subdomain.

---

## 💣 Impact

- Host **malicious content** under a trusted subdomain
- **Phishing attacks** using trusted company domains
- **Cookie theft or session hijacking** via subdomain inclusion
- **Defamation or brand damage**
- **Bypass CSP or SSO domain policies**

---

## 📷 Proof of Concept

**Before Claiming:**

```
404 There isn't a GitHub Pages site here.
```

**After Claiming:**

```
<h1>Subdomain Takeover Test</h1>
```

---

## 🔧 Recommended Remediation

- Remove unused CNAME records from DNS
- Claim GitHub Pages repos/domains proactively
- Monitor DNS records for orphaned services
- Use tools like [`Can I Take Over XYZ`](https://github.com/EdOverflow/can-i-take-over-xyz)

---

## 🔐 Security Best Practices

- Periodically audit DNS records
- Avoid using 3rd-party hosting without ownership checks
- Validate external hosting setups in CI/CD pipelines
- Use DNS monitoring for dangling domains

---

## 🧾 References

- [OWASP - Subdomain Takeover](https://owasp.org/www-community/attacks/Subdomain_takeover)
- [EdOverflow - can-i-take-over-xyz](https://github.com/EdOverflow/can-i-take-over-xyz)
- [GitHub Docs - About GitHub Pages](https://docs.github.com/en/pages)

---

## 📅 Report Date

**2025-06-17**

---

## 🧑‍💻 Reported By

- **Security Researcher**: raghav  
- **Tools Used**: `dig`, GitHub, browser  
- **Severity**: **High**

---

> ⚠️ Kindly review this finding under your responsible disclosure program. Bug bounty reward appreciated if valid.
