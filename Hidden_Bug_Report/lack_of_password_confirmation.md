### 🐞 Bug Report: Lack of Password Confirmation on Critical Account Deletion

**Reported By:** Raghav Arora  
**LinkedIn:** [https://www.linkedin.com/in/iamraghavarora](https://www.linkedin.com/in/iamraghavarora)  
**Severity:** Medium  
**Date Reported:** 2025-06-16

---

### 📄 Summary

The application allows users to **delete their account or perform other critical actions** without requiring a **password re-confirmation**. This lack of re-authentication makes the platform vulnerable to attacks such as **CSRF, session hijacking**, or accidental deletions.

---

### 📌 Affected Component

- Account Settings / Deletion Endpoint  
  *(e.g., `/account/delete`, `/user/remove`)*

---

### 🚨 Proof of Concept (PoC)

**Steps to Reproduce:**

1. Login to the application and navigate to the account settings.
2. Click the "Delete Account" button.
3. Account is deleted **without prompting for password or second confirmation**.

**Observed Result:**
- User account is deleted immediately upon button click or API call.

**Expected Result:**
- System should prompt for **password re-entry** before proceeding with account deletion or critical actions.

---

### 🎯 Impact

- Increases risk of **account takeover exploitation**, where an attacker with temporary or stolen session could delete user account without needing credentials.
- **No safeguard** for accidental clicks or unauthorized access from an active session.
- Weakens **defense-in-depth strategy** for critical action verification.

---

### 🛠️ Suggested Remediation

- Implement **mandatory password confirmation** before account deletion or critical changes (like email, password, or 2FA disabling).
- Consider adding **2FA prompt** if enabled on the account.
- Apply **confirmation dialogs** with context on consequences of action.

---

### 🔒 Security Best Practices

- Always **re-authenticate users** before performing sensitive operations.
- Use **short-lived re-auth tokens** or password fields before final action.
- Ensure **session timeout policies** are enforced.

---

### 🙏 Note to Security Team / Senior Reviewer

Lack of a password prompt on destructive actions significantly reduces security, especially in the event of stolen or hijacked sessions. Enforcing re-authentication is a minimal but effective control to ensure intentional and authenticated actions.
