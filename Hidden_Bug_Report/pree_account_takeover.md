### 🐞 Bug Report: Pre-Account Takeover Vulnerability

**Reported By:** Raghav Arora  
**LinkedIn:** [https://www.linkedin.com/in/iamraghavarora](https://www.linkedin.com/in/iamraghavarora)  
**Severity:** High  
**Date Reported:** 2025-06-16

---

### 📄 Summary

The application is vulnerable to **pre-account takeover**, allowing an attacker to register an account using the email address of a victim **before** the victim signs up. If the victim later attempts to sign up or use a third-party login (e.g., Google, Facebook), the attacker can retain control of that account.

---

### 📌 Affected Component

- User Registration System  
- Email Verification Workflow  
- Third-party OAuth (Social Login) Integration

---

### 🚨 Proof of Concept (PoC)

**Scenario 1: Attacker Registers First**

1. Attacker signs up using victim’s email address (e.g., `victim@example.com`) and sets a password.
2. Victim tries to register later using the same email, or signs in via OAuth (e.g., Google).
3. The app considers the email already registered and may either:
   - Deny the victim access,
   - Or link the attacker’s account to the victim’s OAuth identity, depending on implementation.

**Scenario 2: OAuth Confusion**

1. Attacker registers using `victim@example.com` without email verification.
2. Victim logs in via Google OAuth using the same email.
3. The attacker’s account is now linked with the victim’s Google account (in some misconfigured systems).

**Observed Result:**

- The attacker is able to **claim and control an account intended for the victim**.

**Expected Result:**

- The system should verify **ownership of the email address** **before** account creation is finalized.

---

### 🎯 Impact

- Full **account takeover** of users who haven't yet registered.
- May lead to **unauthorized data access**, impersonation, and **loss of trust**.
- Especially dangerous in applications dealing with sensitive data (banking, health, etc.).

---

### 🛠️ Suggested Remediation

- **Require email verification before account activation**.
- Prevent login (including OAuth-based) until email is verified.
- Use a "pending registration" state until verification is complete.
- For OAuth, ensure that social logins **do not get linked to unverified accounts**.

---

### 🔒 Security Best Practices

- Implement a **robust email verification workflow**.
- Ensure that **OAuth sign-ins validate ownership** and are not merged with unverified email registrations.
- Monitor for **suspicious registrations** using known disposable/targeted domains.

---

### 🙏 Note to Security Team

Pre-account takeover vulnerabilities are subtle but extremely damaging. A single attacker registering a target user's email can lead to downstream risks like impersonation, data loss, and reputational damage. It's crucial to secure account registration flows against this class of attacks.
