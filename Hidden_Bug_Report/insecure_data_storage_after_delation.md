### 🐞 Bug Report: Insecure Data Storage – Accessible Profile Picture After Deletion

**Reported By:** Raghav Arora  
**LinkedIn:** [https://www.linkedin.com/in/iamraghavarora](https://www.linkedin.com/in/iamraghavarora)  
**Severity:** Medium  
**Date Reported:** 2025-06-16

---

### 📄 Summary

The application allows users to upload a profile picture, which is stored publicly. However, **after deleting the profile picture from their account, the uploaded image remains accessible via the original direct URL**. This indicates **insecure data storage** and improper handling of file deletion, leading to potential privacy and data leakage issues.

---

### 📌 Affected Component

- User Profile Picture Upload/Storage System  
- File Deletion Logic

---

### 🚨 Proof of Concept (PoC)

**Steps to Reproduce:**

1. Log in to your account and navigate to the profile section.
2. Upload a new profile picture (e.g., `profile.jpg`).
3. Once uploaded, the file is accessible at a public URL (e.g., `https://example.com/uploads/users/profile.jpg`).
4. Now delete the profile picture from the UI or settings page.
5. Open the same URL in a new tab or via cURL.

**Observed Result:**

The file is still **publicly accessible** via direct link even after deletion.

**Expected Result:**

The file should be **permanently removed** from the server or return a `404 Not Found` response upon deletion.

---

### 🎯 Impact

- **Privacy Violation**: Even after deleting a sensitive photo, it remains accessible.
- **Insecure Data Retention**: Indicates poor backend file management.
- **Compliance Risk**: May violate GDPR or similar data protection laws if users request data deletion.
- **Potential Abuse**: Attackers with cached links can continue to access the media.

---

### 🛠️ Suggested Remediation

- Ensure files are **deleted from the server or storage bucket** when a user deletes them from their profile.
- Add backend logic to unlink and permanently remove the file from storage (e.g., S3, filesystem).
- If soft-delete is used, ensure **access is immediately revoked** post-deletion.
- Implement a **secure media access policy**, such as:
  - Token-based access links
  - Expiry links
  - Permission checks

---

### 🔒 Security Best Practices

- Never store user-uploaded media in permanently public locations unless intended.
- Track file references and remove unused/unlinked files promptly.
- Verify all deletion actions reflect both on the UI and backend.

---

### 🙏 Note to Security Team

This is a classic case of insecure storage due to incomplete deletion logic. Public file hosting should always reflect the current data state visible to the user. In a privacy-respecting application, deletion must mean **complete revocation of access**.
