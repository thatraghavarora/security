### 🐞 Bug Report: No Length Limit on Input Field

**Reported By:** Raghav Arora  
**LinkedIn:** [https://www.linkedin.com/in/iamraghavarora](https://www.linkedin.com/in/iamraghavarora)  
**Severity:** Medium  
**Date Reported:** 2025-06-16

---

### 📄 Summary

The application does **not enforce a maximum character limit** on user input fields (e.g., username, comment, search bar, etc.), allowing excessively long strings to be submitted. This can lead to performance issues, UI breaks, database overflow, or even denial-of-service (DoS) attacks.

---

### 📌 Affected Component

- Input Field: [e.g., Signup Form > "Username", Comment Box, Contact Form, etc.]
- Backend Input Validation
- Frontend Input Attributes

---

### 🚨 Proof of Concept (PoC)

**Steps to Reproduce:**

1. Go to the vulnerable input field (e.g., feedback form).
2. Paste a very large payload, e.g., 10,000+ characters:

   ```text
   AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA... [repeat]
   ```

3. Submit the form.

**Observed Result:**

- The form accepts and processes the large payload without truncation or warning.
- Page or UI may lag, hang, or crash due to rendering too much content.
- Database/storage may store unnecessarily large data entries.

**Expected Result:**

- The input should be limited to a **reasonable character count** (e.g., 100–255 chars).
- The server should reject overly long input with a proper error message.

---

### 🎯 Impact

- **UI/UX disruption:** Long input may overflow or break page layout.
- **Database issues:** Unrestricted input may fill up storage quickly.
- **DoS vulnerability:** Flooding long input repeatedly may impact performance.
- **Log overflow:** Application logs may get bloated with unnecessary data.

---

### 🛠️ Suggested Remediation

- Set maximum character limits both on the **frontend** (`maxlength` attribute in HTML) and **backend/server-side**.
- Validate input length strictly before storing or processing.
- Monitor input lengths through server logs for unusual activity.

---

### 🔒 Security Best Practices

- Limit user input size based on context (e.g., 50 for names, 255 for messages).
- Sanitize and validate all input.
- Consider implementing WAF rules to block unusually large requests.

---

### 🙏 Note to Security Team

Lack of input length validation might seem low risk but can severely affect performance, storage, and user experience. It can also be used in combination with other attacks such as log injection or buffer-related issues. Enforcing proper length limits is a simple yet essential control.
