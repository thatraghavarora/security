### 🐞 Bug Report: Insecure Direct Object Reference (IDOR) – Real World Example

**Reported By:** Raghav Arora  
**LinkedIn:** [https://www.linkedin.com/in/iamraghavarora](https://www.linkedin.com/in/iamraghavarora)  
**Severity:** High  
**Date Reported:** 2025-06-16

---

### 📄 Summary

An **Insecure Direct Object Reference (IDOR)** vulnerability was discovered in the account management functionality. By modifying the user ID parameter in a request, an attacker is able to access, modify, or delete resources belonging to other users **without authorization**.

---

### 📌 Affected Component

- Endpoint: `/api/user/profile/{user_id}`  
- Endpoint: `/download/invoice/{invoice_id}`  
- Endpoint: `/orders/{order_id}/details`

---

### 🚨 Proof of Concept (PoC)

**Scenario 1: Accessing Another User’s Profile**

1. Log in as a normal user (e.g., `user_id = 1002`).
2. Access your profile using:
   ```
   GET /api/user/profile/1002
   ```
3. Now manually change the ID to:
   ```
   GET /api/user/profile/1001
   ```
4. You will receive the profile details of user ID `1001`, even though you are not authorized.

**Scenario 2: Downloading Another User’s Invoice**

1. Logged-in user accesses:
   ```
   GET /download/invoice/84720
   ```
2. Changes the ID to:
   ```
   GET /download/invoice/84719
   ```
3. Another user's sensitive invoice is downloaded without access control.

---

### 🎯 Impact

- Unauthorized access to **sensitive user data**, such as profile information, purchase history, or billing data.
- Could result in **information disclosure**, **account takeovers**, or **unauthorized actions**.
- Severe breach of **user privacy** and **data protection policies**.
- May lead to legal consequences if exposed under data regulations (GDPR, CCPA).

---

### 🛠️ Suggested Remediation

- Implement **authorization checks** on every object access based on session or access token.
- Do not trust user input alone for access control.
- Use **indirect object references** such as UUIDs or hashed values.
- Employ an access control middleware that verifies object ownership before data is returned.
- Regularly **audit endpoints** for ID-based references that lack permission checks.

---

### 🔒 Security Best Practices

- Always validate that the current user has permission to access the resource.
- Avoid exposing sequential IDs if possible.
- Implement **role-based access controls (RBAC)** and **object-level security**.
- Perform **security testing** (manual and automated) for IDOR issues in all CRUD operations.

---

### 🙏 Note to Security Team

This vulnerability demonstrates a critical security oversight in access control. IDOR bugs are commonly exploited in the wild due to their simplicity and high impact. It is recommended to perform a full review of endpoints where user or object IDs are used directly.
