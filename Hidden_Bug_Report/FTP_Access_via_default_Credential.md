### 🐞 Bug Report: FTP Access via Default Credentials (Anonymous Login)

**Reported By:** Raghav Arora  
**LinkedIn:** [https://www.linkedin.com/in/iamraghavarora](https://www.linkedin.com/in/iamraghavarora)  
**Severity:** High  
**Date Reported:** 2025-06-16

---

### 📄 Summary

An FTP server associated with the application is **accessible using default credentials**, specifically **anonymous login** without any authentication. This allows **unauthorized users** to access or modify files on the server.

---

### 📌 Affected Component

- FTP Service  
- Hostname/IP: *(Redacted for security)*  
- Port: 21 (default FTP)

---

### 🚨 Proof of Concept (PoC)

**Steps to Reproduce:**

1. Open a terminal and use the following command to access the FTP server:
   ```bash
   ftp <target-ip>
   ```

2. When prompted for login credentials:
   ```
   Name: anonymous
   Password: anonymous
   ```

3. The server grants access without any restriction.

4. Run `ls` or `dir` to list directory contents.

5. If allowed, try uploading or downloading files:
   ```bash
   put test.txt
   get config.php
   ```

**Observed Result:**

- The FTP server accepts `anonymous` login and grants file listing or file read/write access.

**Expected Result:**

- The FTP server should deny all access without valid credentials and should not allow anonymous login unless explicitly intended and secured.

---

### 🎯 Impact

- **Unauthorized File Access:** Attackers may retrieve sensitive configuration or backup files.
- **File Upload:** Attackers can upload malicious files (e.g., web shells, defacements).
- **Privilege Escalation:** If critical scripts or data are exposed, this could lead to further exploitation.
- **Data Tampering:** The integrity of application data can be compromised.
- **Compliance Risk:** Violates standard security compliance practices (e.g., PCI-DSS, ISO 27001).

---

### 🛠️ Suggested Remediation

- **Disable anonymous FTP login** in the FTP server configuration.
- Require strong, authenticated credentials for FTP access.
- Limit access to specific IP addresses using firewall rules.
- Use **SFTP (Secure FTP)** instead of plain FTP to ensure encrypted transmission.
- Periodically audit FTP logs for unusual or unauthorized activity.

Example (vsftpd configuration):
```bash
anonymous_enable=NO
```

---

### 🔒 Security Best Practices

- Avoid using FTP; prefer SFTP or FTPS.
- Enforce strong authentication and access control.
- Disable write permissions unless necessary.
- Regularly monitor and audit FTP access logs.

---

### 🙏 Note to Security Team / Senior Reviewer

Default or anonymous FTP access is a critical security misconfiguration. This allows external actors to explore and manipulate server data, often without detection. It is strongly recommended to disable anonymous access immediately and review all file transfer protocols in use.
