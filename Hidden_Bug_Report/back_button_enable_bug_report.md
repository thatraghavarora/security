### 🐞 Bug Report: Back Button Functionality Exposes Sensitive Pages

**Reported By:** Raghav Arora  
**LinkedIn:** [https://www.linkedin.com/in/iamraghavarora](https://www.linkedin.com/in/iamraghavarora)  
**Severity:** Medium  
**Date Reported:** 2025-06-16

---

### 📄 Summary

The application allows users to navigate to **previous sensitive pages** (such as login, payment, or logout-success pages) using the browser’s **Back button**. This can lead to **information leakage**, improper session control, or **resubmission of actions** like transactions or forms, especially if proper cache headers and invalidation mechanisms are not applied.

---

### 📌 Affected Component

- Session-sensitive pages (e.g., `/dashboard`, `/checkout/success`, `/logout`)
- Inadequate cache-control and no-store HTTP headers
- Missing page redirects or session invalidation on back navigation

---

### 🚨 Proof of Concept (PoC)

**Steps to Reproduce:**

1. Log in to the application and navigate to a sensitive page (e.g., dashboard).
2. Log out of the application.
3. Press the **Back** button in the browser.
4. Observe that the previous page (dashboard or profile) is still visible.

**Observed Result:**

- The sensitive content is still visible or accessible via the back button.
- No re-authentication prompt is triggered.
- Session state may be restored from browser cache.

**Expected Result:**

- Navigating back should redirect to the login page or show a session timeout message.
- No sensitive content should be rendered after logout.

---

### 🎯 Impact

- **Sensitive Data Disclosure**: Users or attackers on shared systems can view private information after logout.
- **Session Replay Risk**: Previously executed actions may be replayed or resubmitted.
- **Inconsistent User Experience**: May cause confusion or mislead users about session state.
- **Privacy Concerns**: On public/shared machines, private data is still accessible.

---

### 🛠️ Suggested Remediation

- Implement appropriate **cache-control headers**:

```http
Cache-Control: no-cache, no-store, must-revalidate
Pragma: no-cache
Expires: 0
```

- On logout, destroy session data on both client and server.
- Redirect users to the login page on back navigation after logout.
- Use JavaScript to monitor history state and enforce logout redirects if required.

---

### 🔒 Security Best Practices

- Treat all session-sensitive pages as dynamic and private.
- Never cache authenticated content unless explicitly needed and safe.
- Regularly test session management logic against back/forward navigation.

---

### 🙏 Note to Security Team / Senior Reviewer

While this may seem like a UI flaw, improper back button handling can result in serious privacy leaks and security gaps. It is highly recommended to audit the caching policies and ensure proper session invalidation to prevent unauthorized access to protected resources.
