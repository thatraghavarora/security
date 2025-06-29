Here is the **full raw `.md` content** of your bug report with **no extra explanations**, and it includes the note that the endpoint is sometimes blocked but the username can still be found using `?author=1`.

```md
# 🐞 Bug Report – WordPress Username Enumeration via REST API and Author Archives

**Title:** WordPress Usernames Exposed via REST API and Author Archive Enumeration

**Severity:** Medium  
**Vulnerability Type (BugBase.ai):**  
- Information Disclosure  
- User Enumeration  

---

## 📍 Affected URLs:

- `https://example.com/wp-json/wp/v2/users`
- `https://example.com/?author=1` → Redirects to `https://example.com/author/admin`

---

## 📋 Description:

The WordPress instance leaks valid usernames through both the REST API and `?author=1` redirect mechanism. Even when the REST API endpoint is sometimes blocked or returns a `401 Unauthorized`, the `?author=1` method still reveals the username via redirection.

### 1. ✅ REST API Exposure

Request to:

```

GET [https://example.com/wp-json/wp/v2/users](https://example.com/wp-json/wp/v2/users)

````

May return:

```json
[
  {
    "id": 1,
    "name": "Admin",
    "slug": "admin"
  }
]
````

This reveals the slug (`admin`), which is typically the login username.

---

### 2. ✅ Author Archive Enumeration

Request to:

```
https://example.com/?author=1
```

Redirects to:

```
https://example.com/author/admin
```

Revealing the actual username, even if the REST API is blocked.

---

## 💥 Impact:

* Exposes valid usernames used for login
* Increases the risk of:

  * Brute-force login attacks
  * Credential stuffing
  * Targeted phishing or social engineering
* Assists in privilege escalation during chained attacks

---

## 🧪 Steps to Reproduce:

1. Visit `https://example.com/?author=1`
2. Observe redirect to `https://example.com/author/<username>`
3. Optionally test `https://example.com/wp-json/wp/v2/users`
4. Confirm leaked usernames

---

## 🧠 Root Cause:

* Default WordPress settings expose usernames via:

  * Author archives
  * Public REST API
* No mitigation in place to block or hide usernames

---

## ✅ Recommended Fixes:

1. Disable user endpoint in REST API:

   ```php
   add_filter( 'rest_endpoints', function( $endpoints ){
       if ( isset( $endpoints['/wp/v2/users'] ) ) {
           unset( $endpoints['/wp/v2/users'] );
       }
       return $endpoints;
   });
   ```

2. Block author ID-based redirects via `.htaccess`:

   ```apache
   RewriteCond %{QUERY_STRING} author=\d
   RewriteRule ^ /? [L,R=301]
   ```

3. Use plugins like:

   * Stop User Enumeration
   * REST API Toolbox

4. Avoid predictable usernames like `admin`, `editor`, etc.

---

## 📚 References:

* [https://wordpress.org/support/article/hardening-wordpress/](https://wordpress.org/support/article/hardening-wordpress/)
* [https://wordpress.org/plugins/stop-user-enumeration/](https://wordpress.org/plugins/stop-user-enumeration/)
* [https://developer.wordpress.org/rest-api/](https://developer.wordpress.org/rest-api/)

---

## 🙋‍♂️ Notes:

Although this is an information disclosure issue, it contributes to a broader attack surface and facilitates credential-based attacks. The `?author=1` vector makes the enumeration possible even when the REST endpoint is disabled.

```

✅ You can now save this as a `.md` file and submit it directly to any responsible disclosure or bug bounty platform. Let me know if you want this in `.pdf` format or submitted via BugBase or HackerOne template.
```
