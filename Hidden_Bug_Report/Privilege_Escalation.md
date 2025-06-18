# 🐞 Privilege Escalation Vulnerability Report

---

## 📌 Title

**Privilege Escalation via Improper Access Control in Role Assignment Endpoint**

---

## 📝 Description

A privilege escalation vulnerability was discovered in the role management or user update functionality of the web application. Due to missing authorization checks, a low-privileged user (e.g., "basic", "user", or "editor") can escalate their own privileges to administrative roles by directly manipulating request parameters or API endpoints.

This allows unauthorized access to administrative functionality such as user management, content moderation, or sensitive data access.

---

## 🌐 Affected Endpoint

- **URL**: `https://vulnerable-website.com/api/user/update`
- **Method**: `POST`
- **Authentication**: Required (as a regular user)

---

## ⚙️ Vulnerable Request Example

```http
POST /api/user/update HTTP/1.1
Host: vulnerable-website.com
Authorization: Bearer <user-token>
Content-Type: application/json

{
  "userId": "12345",
  "role": "admin"
}
```

✅ The server accepts this request and updates the user's role to **admin** without verifying if the authenticated user has permission to make that change.

---

## 🧪 Steps to Reproduce

### ✅ Pre-requisites

- Have a valid regular user account
- Have access to Burp Suite, Postman, or browser dev tools

### 🔁 Step-by-Step

#### Step 1: Log in as a regular user
- Role: `user`
- Token: obtained via login request

#### Step 2: Intercept or craft the following request

```http
POST /api/user/update HTTP/1.1
Authorization: Bearer <regular-user-token>
Content-Type: application/json

{
  "userId": "12345",
  "role": "admin"
}
```

#### Step 3: Send the request

- If successful, the account is now elevated to `admin`.

#### Step 4: Verify Escalation

Visit an admin-only endpoint:

```http
GET /api/admin/dashboard
Authorization: Bearer <regular-user-token>
```

✅ You now have access to admin-only functionality.

---

## 💣 Impact

- **Privilege Escalation**: Regular users can gain administrative access
- **Account Takeover**: Elevate other users' roles
- **Data Breach**: Access sensitive admin-only data
- **Application Control**: Modify or delete protected resources

---

## 🔧 Recommended Remediation

- **Enforce Role-Based Access Control (RBAC)** on all role-modifying or permission-granting endpoints
- Verify the **current user’s role** before allowing access to any administrative actions
- Never trust input like `role` or `isAdmin` from the client side
- Log and monitor any role change actions

### ✅ Sample Secure Code

```javascript
// Secure check before modifying roles
if (currentUser.role !== 'admin') {
    return res.status(403).json({ error: 'Unauthorized role modification' });
}

// Safe role update
updateUserRole(targetUserId, requestedRole);
```

---

## 🔐 Security Best Practices

- Implement backend-level authorization, not just UI-based restrictions
- Use an access control matrix or permissions map
- Log all privilege changes with user and IP metadata
- Conduct regular audits of user roles and permissions

---

## 🧾 References

- [OWASP Access Control Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Access_Control_Cheat_Sheet.html)
- [CWE-269: Improper Privilege Management](https://cwe.mitre.org/data/definitions/269.html)
- [PortSwigger – Privilege Escalation](https://portswigger.net/web-security/access-control/privilege-escalation)

---

## 📅 Report Date

**2025-06-17**

---

## 🧑‍💻 Reported By

- **Security Researcher**: raghav  
- **Tools Used**: Burp Suite, Manual Testing  
- **Severity**: **Critical**

---

> ⚠️ This issue has been disclosed responsibly. Please confirm the finding and acknowledge the report appropriately.
