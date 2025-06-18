### 🐞 Bug Report: Lack of Password Length Limitation (Excessively Long Passwords Accepted)

**Reported By:** Raghav Arora  
**LinkedIn:** [https://www.linkedin.com/in/iamraghavarora](https://www.linkedin.com/in/iamraghavarora)  
**Severity:** Medium  
**Date Reported:** 2025-06-16

---

### 📄 Summary

The application does not enforce a reasonable **maximum password length** during user registration or login. This allows attackers to input excessively long passwords (e.g., 10,000+ characters), which can lead to **denial of service (DoS)** or resource exhaustion on the server, especially during password hashing operations.

---

### 📌 Affected Component

- User registration and authentication system  
- Password validation and processing logic  

---

### 🚨 Proof of Concept (PoC)

**Steps to Reproduce:**

1. Navigate to the registration or login form.
2. In the password field, enter a string with excessive length (e.g., 50,000+ characters):

```text
AAAAAAAAAAAAAAAA... (repeat 'A' 50,000 times)
```

3. Submit the form.

**Observed Result:**

- The system accepts the password and processes it without any length validation.
- In some cases, the server response is delayed or the application crashes due to resource exhaustion.

---

### ✅ Expected Result

- The system should reject passwords that exceed a secure and reasonable maximum length (e.g., 64–128 characters).
- A proper error message should be displayed to inform the user about password length limits.

---

### 🎯 Impact

- **Denial of Service (DoS):** Long passwords can cause high CPU usage, especially with slow hashing algorithms like `bcrypt`.
- **Server Resource Exhaustion:** May lead to delayed responses or server crashes under multiple simultaneous long-password submissions.
- **Potential Abuse:** Attackers can exploit this in a loop to degrade service availability.

---

### 🛠️ Suggested Remediation

- Enforce a **maximum password length** (recommended: 64–128 characters).
- Perform **input validation on both client-side and server-side**.
- Reject any password exceeding the limit and return a meaningful validation message.
- Example validation rule (in pseudocode):

```python
if len(password) > 128:
    return "Password exceeds maximum length"
```

---

### 🔒 Security Best Practices

- Use secure password hashing algorithms like `bcrypt`, `argon2`, or `scrypt`, but avoid feeding them unbounded-length inputs.
- Document password complexity and length policies in user-facing forms.
- Log attempts with extreme input sizes as potentially malicious behavior.

---

### 🙏 Note to Security Team / Senior Reviewer

Though often overlooked, unbounded password length is a **real attack vector** for DoS and performance degradation. It is important to include a maximum password length policy as part of secure authentication system design.
