
````markdown
# 🐞 CRLF Injection Vulnerability Report

---

## 📌 Title

**CRLF Injection Vulnerability in HTTP Response Headers**

---

## 📝 Description

A **Carriage Return Line Feed (CRLF) Injection** vulnerability was discovered in the target application. This vulnerability allows an attacker to inject arbitrary HTTP headers or manipulate the HTTP response by including special characters (`%0d` for carriage return and `%0a` for line feed) into input fields that are reflected in HTTP response headers.

This can lead to:

- HTTP header injection
- HTTP response splitting
- Session fixation
- Cross-site scripting (XSS)
- Web cache poisoning
- Phishing attacks via content spoofing

---

## 🕵️‍♂️ Vulnerable Parameter

- **Parameter**: `redirect`
- **HTTP Method**: `GET`
- **Endpoint**: `/login`

---

## 📂 Proof of Concept (PoC)

### 🔗 Malicious Request

```http
GET /login?redirect=%0d%0aSet-Cookie:%20crlfInjected=1 HTTP/1.1
Host: vulnerable-website.com
````

### 📥 Response Example

```http
HTTP/1.1 302 Found
Location: /login
Set-Cookie: crlfInjected=1
Content-Type: text/html; charset=UTF-8
...
```

**Observation:**
The application reflects unsanitized input directly into HTTP response headers. As a result, the attacker is able to inject a new header (`Set-Cookie: crlfInjected=1`), which proves that CRLF characters were not sanitized.

---

## 🎯 Impact

* **Header Injection**: Inject arbitrary headers into HTTP response.
* **Session Fixation**: Set cookies to hijack user sessions.
* **Cross-Site Scripting (XSS)**: Indirectly trigger stored or reflected XSS in certain edge cases.
* **Web Cache Poisoning**: Cache is poisoned by manipulated headers, leading to cached malicious responses.
* **HTTP Response Splitting**: Break and control HTTP responses leading to content injection or phishing.

---

## 🧪 Steps to Reproduce

1. Open a browser or proxy tool such as **Burp Suite**.
2. Send a request to the vulnerable URL:

```perl
https://vulnerable-website.com/login?redirect=%0d%0aSet-Cookie:%20hacked=1
```

3. Observe the server response.
4. Note that the following header is now part of the HTTP response:

```javascript
Set-Cookie: hacked=1
```

This confirms that CRLF characters were interpreted and new headers were injected.

---

## ⚙️ Affected Components

* HTTP response handling
* URL redirection logic
* Header reflection mechanisms
* Any feature that directly inserts user input into headers

---

## 🛡️ Recommended Remediation

* **Input Validation**: Reject or sanitize input containing CR (`\r`) or LF (`\n`) characters.
* **Encoding**: Encode user input before placing it into HTTP response headers.
* **Use Framework Methods**: Always use built-in secure methods for setting headers or redirects.
* **Safe Allowlist**: Implement a strict allowlist for redirect URLs or reflected input.

---

## 🔧 Example Fix (Pseudocode)

```python
import urllib.parse

def safe_redirect(url):
    if contains_crlf(url):
        return error_page("Invalid input")
    return redirect(urllib.parse.quote(url))

def contains_crlf(input_string):
    return '\r' in input_string or '\n' in input_string
```

---

## 🔐 Security Best Practices

* Never trust user-supplied input.
* Use proper output encoding for HTTP headers and content.
* Implement security headers:

  * `Content-Security-Policy`
  * `Strict-Transport-Security`
  * `X-Content-Type-Options: nosniff`
  * `X-Frame-Options: DENY`
* Regularly audit applications with automated tools and manual testing.
* Log and monitor unusual input patterns in headers.

---

## 🧾 References

* [OWASP: HTTP Response Splitting](https://owasp.org/www-community/attacks/HTTP_Response_Splitting)
* [PortSwigger: CRLF Injection](https://portswigger.net/web-security/response-splitting)

---

## 📅 Report Date

**2025-06-17**

---

## 🧑‍💻 Reported By

* **Security Researcher**: raghav
* **Tools Used**: Burp Suite, Manual Testing
* **Severity**: High

---

> ✅ **Disclaimer:** This vulnerability report was written for responsible disclosure purposes.
