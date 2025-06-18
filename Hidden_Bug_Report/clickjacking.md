### 🐞 Bug Report: Clickjacking Vulnerability

**Reported By:** Raghav Arora  
**LinkedIn:** [https://www.linkedin.com/in/iamraghavarora](https://www.linkedin.com/in/iamraghavarora)  
**Severity:** High  
**Date Reported:** 2025-06-16

---

### 📄 Summary

The target application is vulnerable to **Clickjacking**, a UI redress attack where a malicious website can embed the target website within an iframe and trick users into interacting with UI elements (such as buttons or forms) without their knowledge or consent.

---

### 📌 Affected Component

- Web application pages that allow framing by other domains.
- Absence of proper security headers like `X-Frame-Options` or `Content-Security-Policy`.

---

### 🚨 Proof of Concept (PoC)

**Steps to Reproduce:**

1. Create a malicious HTML file on an attacker-controlled domain:
    ```html
    <html>
    <head>
      <title>Clickjacking PoC</title>
    </head>
    <body>
      <h1>Click anywhere to win a prize!</h1>
      <iframe src="https://target-site.com/transfer" style="opacity:0; position:absolute; top:0; left:0; width:100%; height:100%; z-index:999;"></iframe>
    </body>
    </html>
    ```

2. Host the file and trick a logged-in user to visit this page.
3. When the user clicks thinking they’re interacting with the attacker’s content, they are actually clicking on sensitive actions within the target application (e.g., transferring funds, deleting data).

**Observed Result:**

- The legitimate page from the target site is loaded invisibly via an iframe.
- The user unknowingly interacts with it, performing unintended actions.

**Expected Result:**

- The application should **not allow itself to be loaded inside iframes** on external domains.

---

### 🎯 Impact

- **Sensitive Action Hijacking:** Users may unknowingly perform dangerous actions (e.g., logout, delete, transfer).
- **Loss of Trust:** Attackers can create deceptive overlays or phishing content.
- **Brand Abuse:** Malicious parties can frame your UI in deceptive contexts.
- **Regulatory Violation:** Could lead to unauthorized access or action violations.

---

### 🛠️ Suggested Remediation

1. Set the `X-Frame-Options` header:
    ```
    X-Frame-Options: DENY
    ```
    Or (if you need to allow framing on the same origin):
    ```
    X-Frame-Options: SAMEORIGIN
    ```

2. Use a `Content-Security-Policy` frame-ancestor directive:
    ```
    Content-Security-Policy: frame-ancestors 'none';
    ```

3. Validate and secure all endpoints performing sensitive actions with **CSRF protection**, **user confirmation**, and **authentication**.

---

### 🔒 Security Best Practices

- Implement frame-busting techniques and headers.
- Regularly audit endpoints for unintended UI exposure.
- Educate users on avoiding phishing/clickjacking attempts.

---

### 🙏 Note to Security Team / Senior Reviewer

Clickjacking is a serious security concern, especially for applications that perform sensitive actions based on user clicks. The lack of `X-Frame-Options` or CSP headers leaves the application open to invisible interaction hijacking. Remediation is straightforward and critical for user protection.
