### 🐞 Bug Report: Incomplete Session Invalidation Across Browsers

**Reported By:** Raghav Arora  
**LinkedIn:** [https://www.linkedin.com/in/iamraghavarora](https://www.linkedin.com/in/iamraghavarora)  
**Severity:** High  
**Date Reported:** 2025-06-16

---

### 📄 Summary

The application fails to **invalidate all active sessions** when a user logs out from one browser. This allows the same user session to remain active and fully usable in another browser or device, indicating a lack of proper session invalidation and centralized session management.

---

### 📌 Affected Component

- Session management and logout mechanism
- User authentication system

---

### 🚨 Proof of Concept (PoC)

**Steps to Reproduce:**

1. Log into the application in **Browser A**.
2. Copy the session token (if accessible) or log in with the same account in **Browser B**.
3. Log out from **Browser A**.
4. Observe that **Browser B** still has full access to the account.

**Observed Result:**

- Logging out from one browser **does not invalidate** sessions in other browsers.
- All previous sessions remain **active and valid**, even after logout.

**Expected Result:**

- Logging out from any browser should **invalidate the session globally**, ensuring all instances are logged out.
- All active tokens associated with that user should be destroyed or marked as invalid.

---

### 🎯 Impact

- **Account Takeover Risk**: If session tokens are stolen or shared, attackers retain access even if the victim logs out.
- **Session Hijacking**: Valid sessions persist in other browsers, increasing risk.
- **Inconsistent Security Behavior**: Users may assume logout ends all sessions, creating a false sense of security.
- **Non-compliance** with security standards like OWASP recommendations or enterprise SSO policies.

---

### 🛠️ Suggested Remediation

- Implement **centralized session management** that tracks all active sessions per user.
- On logout, **invalidate all associated session tokens** (unless using specific session retention features).
- Store user sessions in a database or session store and mark them invalid during logout.
- Use secure token lifecycle controls (e.g., JWT blacklisting or session revocation).

---

### 🔒 Security Best Practices

- Always implement global logout functionality for sensitive applications.
- Provide users with a **“Log out from all devices”** feature.
- Regularly expire sessions based on inactivity or maximum lifetime.
- Monitor and audit all active sessions per user with IP and device fingerprinting.

---

### 🙏 Note to Security Team / Senior Reviewer

This issue reflects a failure to properly enforce **session invalidation across devices**, which undermines the integrity of the logout functionality. It is recommended to implement full-session tracking and destroy all tokens upon logout unless explicitly whitelisted under secure policy (e.g., trusted device).
