### 🐞 Bug Report: Duplicate Account Creation Using Same Email (Email + OAuth)

**Reported By:** Raghav Arora  
**LinkedIn:** [https://www.linkedin.com/in/iamraghavarora](https://www.linkedin.com/in/iamraghavarora)  
**Severity:** High  
**Date Reported:** 2025-06-16

---

### 📄 Summary

The application allows users to create **two different accounts** using the **same email address**, one via normal registration and one via OAuth (e.g., Google Login). These accounts are treated as separate users, which leads to a **broken identity model**, **bypasses email verification**, and can result in **account confusion or takeover**.

---

### 📌 Affected Component

- User Registration System  
- OAuth Integration (Google, Facebook, etc.)  
- Email Verification & Account Linking Logic  

---

### 🚨 Proof of Concept (PoC)

**Steps to Reproduce:**

1. Register on the site using a normal email/password sign-up with the email `victim@example.com`.
2. Do not complete email verification.
3. Now use the "Login with Google" option using the same email `victim@example.com`.
4. Observe that a **new account is created** and logged in **without requiring email verification**.

**Observed Behavior:**

- Two different user accounts are created with the same email address.
- The OAuth account bypasses email verification.
- Both accounts are treated as separate users in the system.

**Expected Behavior:**

- If an account with the same email already exists (even unverified), OAuth login should either:
  - Link to the existing account and require verification, or
  - Block registration until the original email is verified.

---

### 🎯 Impact

- Bypasses email verification workflow.
- Enables attackers to create duplicate or fake accounts.
- Can result in **user confusion** or **privilege escalation** in multi-user systems.
- Can potentially allow **account takeover** if security flows (like password reset) are email-based.
- Inconsistent account behavior may damage user trust and data integrity.

---

### 🛠️ Suggested Remediation

- Implement a **check for existing accounts** based on email before creating a new one through OAuth.
- Enforce **account linking** instead of separate account creation.
- Require email ownership confirmation even with OAuth sign-ins.
- Prevent duplicate accounts with the same email address regardless of authentication method.

---

### 🔒 Security Best Practices

- Ensure **one unique user per verified email address**.
- Maintain consistent account state across all login methods.
- Use secure linking mechanisms for OAuth and existing accounts (e.g., confirmation prompts).
- Log and monitor account creation flows for anomalies.

---

### 🙏 Note to Security Team

This issue may appear harmless but has serious long-term implications. OAuth should not be treated as a verified identity source unless explicitly validated. Duplicate accounts with the same email lead to broken authentication and misaligned access control.
