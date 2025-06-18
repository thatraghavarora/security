### 🐞 Bug Report: Missing Rate Limit on Multi-File Upload Feature

**Reported By:** Raghav Arora  
**LinkedIn:** [https://www.linkedin.com/in/iamraghavarora](https://www.linkedin.com/in/iamraghavarora)  
**Severity:** High  
**Date Reported:** 2025-06-16

---

### 📄 Summary

The application allows users to upload **an unlimited number of files** through the multi-file upload feature, **without any rate-limiting or upload cap**. This opens the application to **abuse**, **storage exhaustion**, and **denial of service (DoS)** scenarios.

---

### 📌 Affected Component

- File Upload Section supporting multiple file uploads
- Backend file upload handler or API

---

### 🚨 Proof of Concept (PoC)

**Steps to Reproduce:**

1. Go to the multi-file upload section of the application.
2. Select and upload **hundreds or thousands of files** using the uploader.
3. Observe that the system accepts all uploads **without rejection or throttling**.

**Observed Result:**

- There is no restriction on the **number of files** that can be uploaded.
- No **rate limiting**, **upload cap**, or **queue limit** is enforced.
- Application continues accepting an unlimited stream of uploads.

**Expected Result:**

- The application should limit the number of files uploaded per request (e.g., max 5–10 files).
- Enforce **rate limiting per IP/user/session** to prevent abuse.
- Large bursts of uploads should be **throttled** or temporarily blocked.

---

### 🎯 Impact

- **Storage Exhaustion:** Attackers can fill server disk space quickly.
- **Denial of Service:** Continuous bulk uploads may crash the server or slow it down.
- **Increased Costs:** In cloud environments, this may increase bandwidth/storage costs.
- **Potential Bypass of Moderation:** Spammers may upload thousands of harmful/illegal files.

---

### 🛠️ Suggested Remediation

- Limit number of files per upload (e.g., 5 files max per form submission).
- Implement **rate limiting** per IP address or authenticated user.
- Introduce backend throttling logic (e.g., using Redis-based counters or rate-limiting middleware).
- Show clear UI messages for rejected or excessive uploads.
- Monitor upload activity for abuse patterns.

**Example (Node.js Express):**

```js
const rateLimit = require("express-rate-limit");

const uploadLimiter = rateLimit({
  windowMs: 5 * 60 * 1000, // 5 minutes
  max: 10, // limit each IP to 10 uploads per window
  message: "Too many files uploaded, please try again later.",
});

app.post('/upload', uploadLimiter, uploadHandler);
```

---

### 🔒 Security Best Practices

- Combine file count, size, and rate limits for robust file upload security.
- Always validate user role and authentication before accepting files.
- Queue and scan uploaded files for moderation if needed.
- Use logging to detect abuse and auto-block IPs or users exceeding thresholds.

---

### 🙏 Note to Security Team / Senior Reviewer

The absence of rate limits on bulk upload functionality poses a serious risk. It makes the application vulnerable to **resource abuse**, **platform degradation**, and **malicious mass uploads**. Adding proper upload caps and throttling will significantly strengthen the platform’s resilience.
