### 🐞 Bug Report: Business Logic Vulnerability in Discount Application

**Reported By:** Raghav Arora  
**LinkedIn:** [https://www.linkedin.com/in/iamraghavarora](https://www.linkedin.com/in/iamraghavarora)  
**Severity:** High  
**Date Reported:** 2025-06-16  

---

### 📄 Summary

A **business logic flaw** was discovered in the discount coupon system of the e-commerce platform. Users are able to **apply the same discount code multiple times** or in unintended scenarios, resulting in **unauthorized financial benefits** and potential loss to the business.

---

### 📌 Affected Component

- Checkout Process  
- Discount Code Logic  
- Cart Validation API  

---

### 🚨 Proof of Concept (PoC)

**Steps to Reproduce:**

1. Add any product to the cart.
2. Apply a one-time discount coupon (e.g., `DISCOUNT50`).
3. Intercept the request applying the coupon via proxy (like Burp Suite).
4. Replay the same request multiple times or refresh the page.
5. Observe that the discount is repeatedly applied, exceeding its intended value.

**Observed Result:**
- Discount gets applied multiple times.
- Final cart value becomes zero or negative in some cases.

**Expected Result:**
- Discount should be applied only once per user or order.
- Backend should enforce single-use restrictions.

---

### 🎯 Impact

- Financial loss due to repeated discounts.
- Bypass of marketing and business limitations.
- Inventory manipulation if free/discounted items are repeatedly ordered.
- Abuse by automated scripts or bots.

---

### 🛠️ Suggested Remediation

- Implement **server-side logic** to restrict coupon usage:
  - Limit per user/session/order.
  - Track usage count in database.
- Add **flags** for one-time use and enforce checks before applying discounts.
- Implement **rate-limiting** and bot detection on discount endpoints.

---

### 🔒 Security Best Practices

- Always validate business logic **on the backend**.
- Test for edge cases like replay, refresh, and concurrent requests.
- Ensure **integrity checks** between UI and backend business rules.

---

### 🙏 Note to Security Team

Business logic vulnerabilities are subtle but can cause major damage. They often bypass traditional security scanners, so it’s critical to manually test flows that involve **financial transactions, user limits, or resource control**.
