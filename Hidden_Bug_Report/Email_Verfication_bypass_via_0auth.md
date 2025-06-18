### 🐞 Bug Report: Email Verification Bypass via OAuth Login

**Reported By:** Raghav Arora  
**LinkedIn:** [https://www.linkedin.com/in/iamraghavarora](https://www.linkedin.com/in/iamraghavarora)  
**Severity:** High  
**Date Reported:** 2025-06-16

---

### 📄 Summary

A vulnerability was discovered that allows users to bypass **email verification** by signing up or logging in via a **third-party OAuth provider** (e.g., Google, Facebook, GitHub). This flaw enables attackers to access authenticated areas of the application without confirming ownership of their email address.

---

### 📌 Affected Component

- **User registration & login module**
- **Email verification workflow**
- **OAuth integration (Social Login APIs)**

---

### 🚨 Proof of Concept (PoC)

**Steps to Reproduce:**

1. Start the normal registration process on the website.
2. Notice the site requires email verification via a link sent to the user's inbox.
3. Instead of completing the verification step, use a social login provider (e.g., "Login with Google").
4. If the OAuth email matches the unverified email, access is granted immediately **without email verification**.

**Result:**  
User is logged in with an unverified email and granted access to features reserved for verified users.

---

### 🎯 Impact

- **Bypasses security measures** that depend on verified emails (e.g., access control, notifications, password resets).
- **Spam or fake accounts** can be created rapidly without proper validation.
- **Privilege escalation** in systems that treat OAuth login as automatically trusted.
- In platforms using email-based identity for permissions, **access control violations** may occur.

---

### 🛠️ Suggested Remediation

- ✅ Ensure that email verification is **enforced separately**, even after OAuth login.
- ✅ Introduce a **post-OAuth verification step** for first-time users.
- ✅ Verify that the email received from OAuth matches a verified user in the system.
- ✅ Deny access to restricted features until email status is marked as `verified`.

---

### 🔒 Security Best Practices

- Treat OAuth logins as **authentication** but not **identity validation** unless domain and email are verified.
- Mark all OAuth-linked accounts as `unverified` by default until verification is completed.
- Show warning messages or limit functionality for users with unverified emails.

---

### 🙏 Note to Security Team / Developers

OAuth simplifies sign-in but should not replace critical email verification workflows. Without an extra layer of confirmation, attackers can exploit social login to bypass protections meant to ensure user authenticity and communication reliability.
