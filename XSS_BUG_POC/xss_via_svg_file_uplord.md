### 🐞 Bug Report: Stored XSS via Malicious SVG File Upload

**Reported By:** Raghav Arora  
**LinkedIn:** [https://www.linkedin.com/in/iamraghavarora](https://www.linkedin.com/in/iamraghavarora)  
**Severity:** High  
**Date Reported:** 2025-06-16

---

### 📄 Summary

A **Stored Cross-Site Scripting (XSS)** vulnerability was identified via uploading a **malicious SVG file**. Since SVG is an XML-based image format that supports JavaScript, uploading and rendering such a file in the browser without sanitization can lead to script execution.

---

### 📌 Affected Component

- **File Upload Endpoint**
- **Media Preview/Viewer Page** that renders uploaded SVGs

---

### 🚨 Proof of Concept (PoC)

**Steps to Reproduce:**

1. Create a malicious SVG file (`xss.svg`) with the following content:

   ```xml
   <svg xmlns="http://www.w3.org/2000/svg">
     <script>alert('XSS via SVG')</script>
   </svg>
   ```

2. Upload the file through the application's file upload functionality.
3. Navigate to the page that displays the uploaded file.

**Observed Result:**

- The browser executes the embedded script and shows an alert: `XSS via SVG`.

**Expected Result:**

- The SVG should be sanitized or downloaded as a file, not rendered inline with active JavaScript.

---

### 🎯 Impact

- Allows **Stored XSS** affecting anyone who views the rendered SVG.
- Could result in:
  - **Session hijacking**
  - **Credential theft**
  - **Admin panel takeover**
  - **Drive-by attacks**

---

### 🛠️ Suggested Remediation

- **Disallow SVG uploads** if not needed.
- If SVGs must be allowed:
  - Sanitize them using tools like [SVGO](https://github.com/svg/svgo) or [DOMPurify](https://github.com/cure53/DOMPurify).
  - Serve uploaded SVG files with `Content-Disposition: attachment` headers to force download.
  - Avoid rendering user-uploaded SVGs inline.

---

### 🔒 Security Best Practices

- Treat SVG files as **potentially dangerous** content.
- Store and serve user-generated files from a different domain/subdomain to reduce the attack surface.
- Apply strong **Content Security Policy (CSP)** to restrict script execution.

---

### 🙏 Note to Security Team / Developers

This vulnerability highlights the risks of rendering user-uploaded vector files directly in the DOM. Due to SVG’s ability to include scripts and external references, it's important to sanitize or sandbox such content to avoid XSS attacks.
