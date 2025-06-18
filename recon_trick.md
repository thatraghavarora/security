# 🔍 Complete Web Recon Guide: Terminal + Browser Extensions Workflow

## 🎯 Goal

Perform effective web recon using:
- 🔧 Terminal (only for subdomain enumeration & filtering)
- 🌐 Browser extensions and web-based tools (for scanning, fingerprinting, and vulnerability detection)

---

## 🧑‍💻 Step 1: Subdomain Enumeration (Terminal-Based)

### 🔹 Tools Required

- [subfinder](https://github.com/projectdiscovery/subfinder)
- [httpx](https://github.com/projectdiscovery/httpx)

### 🔹 Commands to Run

```bash
# Create directory
mkdir recon && cd recon

# Step 1: Find subdomains
subfinder -d target.com -silent > subs.txt

# Step 2: Check status code of subdomains
cat subs.txt | httpx -status-code -silent > filtered.txt

# Step 3: Split into categories
cat filtered.txt | grep "\[200\]" > subdomains_200.txt
cat filtered.txt | grep "\[403\]" > subdomains_403.txt
cat filtered.txt | grep "\[404\]" > subdomains_404.txt
```

---

## 🌐 Step 2: Open Subdomains in Browser

### 🔹 Use "Open Multiple URLs" Extension

1. Copy contents of `subdomains_200.txt`, `subdomains_403.txt`, or others
2. Install extension:  
   👉 [Open Multiple URLs (Chrome)](https://chrome.google.com/webstore/detail/open-multiple-urls)
3. Paste and open all links in new tabs

---

## 🧩 Step 3: Install Browser Extensions

### 🧰 Recommended Extensions List

| Extension                             | Purpose                                |
| ------------------------------------- | -------------------------------------- |
| **Shodan**                            | Check for exposed services, open ports |
| **Wappalyzer**                        | Detect technologies and CMS used       |
| **.DotGit**                           | Check for exposed `.git` repos         |
| **IP/DNS & Security Tools**           | WHOIS, DNS, ping, traceroute           |
| **JavaScript Vulnerability Detector** | Finds vulnerable JS libs               |
| **Link Gopher**                       | Extracts all links for testing         |
| **Retire.js**                         | Detects outdated JS libraries          |
| **Trufflehog**                        | Find secrets in JS or exposed repos    |
| **NoScript Suite Lite**               | JS/XSS behavior analysis               |
| **OWASP PenTesting Kit**              | Set of in-browser testing tools        |
| **AutoFill Extension**                | Auto-fills forms with custom payloads  |
| **D3coder**                           | Decode Base64, URL-encoded, Hex, etc.  |

---

## 🔄 Step 4: Observe and Analyze (Manual Recon)

1. Browse each subdomain manually
2. Use installed extensions to:

   * Detect technologies
   * Check for open `.git` folders
   * Detect outdated or vulnerable libraries
   * Analyze JavaScript activity
   * Look for broken links
   * Auto-fill payloads on forms
   * Decode encoded strings (D3coder)

---

## 🛠️ Step 5: Advanced Web Checks (Browser-Based)

Use the following online tools for further recon:

| Check                     | Tool/URL Example                                               |
|--------------------------|---------------------------------------------------------------|
| **CORS Misconfig**       | [https://gf.dev/cors-test](https://gf.dev/cors-test)          |
| **Iframe Injection**     | [https://www.toolsvoid.com/test-iframe](https://toolsvoid.com/test-iframe/) |
| **Security Headers**     | [https://securityheaders.com/](https://securityheaders.com/)  |
| **HSTS Preload Status**  | [https://hstspreload.org/](https://hstspreload.org/)          |
| **SPF Check**            | [https://mxtoolbox.com/spf.aspx](https://mxtoolbox.com/spf.aspx) |
| **DMARC Check**          | [https://mxtoolbox.com/DMARC.aspx](https://mxtoolbox.com/DMARC.aspx) |
| **DKIM Check**           | [https://mxtoolbox.com/dkim.aspx](https://mxtoolbox.com/dkim.aspx) |

✅ **Tip**: You can copy all subdomains and use browser extensions like *Open Multiple URLs* to test them in these tools quickly.

---

## 📝 Final Step: Reporting

Once a bug or misconfiguration is found:

- ✅ Capture request/response headers
- ✅ Save PoC screenshots or videos
- ✅ Document steps to reproduce
- ✅ Report via bug bounty platform, email, or responsible disclosure form

---

## ✅ Summary: Workflow

```text
[ Terminal ]
↓
Find Subdomains → Check HTTP Status → Output Subdomains with 200/403/404
↓
[ Browser ]
Paste subdomains → Open using "Open Multiple URLs"
↓
Use Extensions → Manual Recon
↓
Online Tools → Check CORS, Iframes, Headers, SPF, DKIM
↓
Document and Report
```

---

## 📦 Output Files Structure

| File Name            | Description                 |
| -------------------- | --------------------------- |
| `subs.txt`           | All discovered subdomains   |
| `filtered.txt`       | Subdomains with HTTP status |
| `subdomains_200.txt` | Accessible (200 OK) domains |
| `subdomains_403.txt` | Forbidden pages (403)       |
| `subdomains_404.txt` | Not found pages (404)       |

---

> ⚠️ **Disclaimer:** Use this guide only for ethical hacking and authorized testing. Do not test without proper permission.

