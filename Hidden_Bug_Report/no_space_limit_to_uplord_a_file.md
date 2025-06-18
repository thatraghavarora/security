### 🐞 Bug Report: Unrestricted File Upload Size (No File Size Limit)

**Reported By:** Raghav Arora  
**LinkedIn:** [https://www.linkedin.com/in/iamraghavarora](https://www.linkedin.com/in/iamraghavarora)  
**Severity:** Medium to High  
**Date Reported:** 2025-06-16

---

### 📄 Summary

The application does **not enforce a maximum file upload size**, allowing users to upload files larger than **1 GB**. This lack of restriction can lead to **resource exhaustion**, **denial of service (DoS)** conditions, **storage abuse**, and degraded application performance.

---

### 📌 Affected Component

- File upload endpoint (e.g., profile picture upload, document upload)
- Server-side upload handler (API or form action)

---

### 🚨 Proof of Concept (PoC)

**Steps to Reproduce:**

1. Prepare any large file (e.g., 1.5 GB `.mp4`, `.zip`, or `.iso`).
2. Go to the file upload section of the application.
3. Upload the large file via the upload feature.
4. Observe that the file is uploaded successfully with no error or size restriction.

**Observed Result:**

- The application accepts files of extremely large sizes (e.g., 1 GB+).
- No warnings or limits are presented to the user.
- Server and application performance may degrade or crash.

**Expected Result:**

- The application should **enforce a file size limit** (e.g., 10 MB for images, 100 MB for documents).
- Uploads exceeding the limit should be **blocked and return an error**.

---

### 🎯 Impact

- **Denial of Service (DoS):** Multiple large uploads can exhaust server memory, disk space, or bandwidth.
- **Storage Abuse:** Malicious users can fill server storage with junk files.
- **Application Crash:** Some systems may crash if unable to handle such large files.
- **Increased Costs:** Cloud storage and bandwidth may increase significantly with abuse.
- **Service Degradation:** Legitimate users may face performance issues due to server overload.

---

### 🛠️ Suggested Remediation

- **Set maximum file size limits** on both client and server side:
  - Frontend validation (e.g., HTML5 `max-size` attribute)
  - Backend configuration (e.g., PHP: `upload_max_filesize`, Node: `limit` in body parser)
- Reject requests exceeding the limit with clear error messages.
- Log attempts to upload large files for monitoring potential abuse.

**Example (PHP):**

```ini
upload_max_filesize = 20M
post_max_size = 25M
```

**Example (Express.js):**

```js
app.use(express.json({ limit: '20mb' }));
```

---

### 🔒 Security Best Practices

- Always enforce file size limits both client-side and server-side.
- Monitor disk usage and alert on unusual upload patterns.
- Use file compression or chunking for large legitimate uploads.
- Store large uploads in scalable storage (e.g., AWS S3) with validation controls.

---

### 🙏 Note to Security Team / Senior Reviewer

While this issue may not allow code execution or direct takeover, it presents a clear path to **abuse application resources**, especially in multi-user environments or public upload portals. Limits should be implemented and clearly communicated to end users to protect the system's integrity.
