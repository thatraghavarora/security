### 🐞 Bug Report: Weak Session Management

**Reported By:** Raghav Arora  
**LinkedIn:** [https://www.linkedin.com/in/iamraghavarora](https://www.linkedin.com/in/iamraghavarora)  
**Severity:** High  
**Date Reported:** 2025-06-16

---

### 📄 Summary

The application demonstrates **insecure session management practices**, which can allow attackers to **hijack sessions**, maintain unauthorized access, or **bypass authentication mechanisms**. A secure web application should handle session tokens securely and invalidate them upon logout or timeout.

---

### 📌 Affected Components

- Session token handling (cookies or headers)
- Logout and session expiration behavior
- Session ID regeneration on login

---

### 🚨 Proof of Concept (PoC)

**Observed Issues:**

1. **Session Not Invalidated on Logout**  
   After logging out, using the browser back button or resending a previously valid request still gives access to authenticated pages.

2. **Predictable or Non-Rotated Session IDs**  
   The session ID is not regenerated upon login. This allows an attacker to reuse an old session or exploit session fixation.

3. **Session Tokens Without Secure Flags**  
   Cookies lack `HttpOnly`, `Secure`, or `SameSite` attributes, exposing them to XSS or cross-site request attacks.

---

### ✅ Expected Result

- Session should be destroyed on logout.
- Session IDs must be rotated on login and sensitive actions.
- Session cookies must be set with:
  - `HttpOnly`
  - `Secure` (when over HTTPS)
  - `SameSite=Strict` or `Lax`

---

### 🎯 Impact

- **Session Hijacking**: Attackers can reuse valid tokens.
- **Session Fixation**: If the session ID does not change, an attacker can log in with a known session.
- **Unauthorized Access**: Re-accessing protected pages post-logout is a privacy violation.
- **Increased Attack Surface**: Tokens are accessible via JavaScript and may be stolen via XSS.

---

### 🛠️ Suggested Remediation

- Invalidate sessions on logout by destroying the session token server-side.
- Rotate session identifiers on login and privilege changes.
- Set secure cookie attributes:

```http
Set-Cookie: sessionid=xyz; HttpOnly; Secure; SameSite=Strict
```

- Implement idle and absolute session timeouts (e.g., 15 mins of inactivity or 1-hour max session).

---

### 🔒 Security Best Practices

- Never expose session tokens to client-side scripts.
- Avoid storing sessions in localStorage/sessionStorage.
- Regularly audit session handling across all endpoints.
- Ensure logout functionality completely removes session state.

---

### 🙏 Note to Security Team / Senior Reviewer

Weak session management is a critical class of vulnerabilities that directly impact the confidentiality and integrity of user accounts. Improving session token handling should be prioritized to prevent hijacking and ensure compliance with secure development guidelines.
