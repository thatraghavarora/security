### 🐞 Bug Report: Race Condition Vulnerability in Critical Operation

**Reported By:** Raghav Arora  
**LinkedIn:** [https://www.linkedin.com/in/iamraghavarora](https://www.linkedin.com/in/iamraghavarora)  
**Severity:** High  
**Date Reported:** 2025-06-16

---

### 📄 Summary

A **race condition vulnerability** was discovered in the application's critical operation endpoint (e.g., balance transfer, coupon redemption, or inventory purchase). This issue allows multiple parallel requests to manipulate the system state in an unintended way, resulting in **duplicate actions**, **privilege abuse**, or **financial gain**.

---

### 📌 Affected Component

- Transactional or critical update endpoints (e.g., `/redeem`, `/withdraw`, `/checkout`)  
- Operations that involve state changes based on user input (e.g., wallet deduction, inventory count)

---

### 🚨 Proof of Concept (PoC)

**Steps to Reproduce:**

1. Login to the user account.
2. Identify a critical operation (e.g., redeeming a coupon, transferring funds).
3. Use tools like **Burp Suite Intruder**, **Turbo Intruder**, or **custom scripts** to send **multiple simultaneous requests** to the vulnerable endpoint.
4. Observe that **multiple operations succeed**, despite expecting only one successful outcome.

**Example Request:**
```http
POST /api/redeem-coupon HTTP/1.1
Host: vulnerable-site.com
Authorization: Bearer <token>
Content-Type: application/json

{
  "coupon_code": "WELCOME100"
}
```

**Exploit:**  
Sending 10 concurrent requests leads to **multiple redemptions** of the same coupon.

---

### ❗ Observed Result

- More than one action is successfully executed (e.g., coupon redeemed twice, multiple balances credited/deducted, item purchased multiple times).
- No locking or transaction checks are in place to prevent concurrent access.

**Expected Result:**
- Only one operation should succeed.
- Subsequent or simultaneous requests should be blocked, locked, or rejected.

---

### 🎯 Impact

- **Financial loss** (multiple credits or redemptions).
- **Inventory manipulation** (double-purchase of limited stock items).
- **Privilege abuse** by attackers automating this flaw.
- **System state corruption** if parallel updates are not atomic.

---

### 🛠️ Suggested Remediation

- Implement **atomic operations** using database-level transactions or locks.
- Use flags or tokens (e.g., one-time-use IDs) to ensure **idempotency**.
- Add **backend validation** to prevent multiple executions of the same action.
- Use **synchronization techniques** such as mutexes, semaphores, or optimistic concurrency control.

---

### 🔒 Security Best Practices

- Treat all critical state-changing operations as **non-idempotent** and protect them accordingly.
- Always validate input **against current state** before processing.
- Log concurrent access attempts and **rate-limit** suspicious behavior.

---

### 🙏 Note to Security Team / Senior Reviewer

Race conditions often go unnoticed until they're exploited. This vulnerability introduces real risk, especially in financial or transactional flows. Implementing atomicity and state validation is crucial to maintaining system integrity.
