# Blind SQL Injection

## 📌 Overview

Blind SQL Injection occurs when an application is vulnerable to SQL injection but does not directly display database errors or query results to the attacker.

Instead of retrieving data directly, attackers infer information by observing:

* Application responses
* Boolean conditions
* Response timing
* Error behavior
* Out-of-band interactions

Blind SQL injection is commonly found in real-world applications because developers suppress database errors.

---

# 🧠 What is Blind SQL Injection?

Blind SQL Injection happens when user-controlled input modifies SQL queries, but the application does not visibly return database output.

Attackers extract information indirectly by sending carefully crafted payloads and analyzing server behavior.

---

# 🔥 Types of Blind SQL Injection

## Boolean-Based Blind SQLi

The application behaves differently depending on whether a condition is TRUE or FALSE.

Example:

```sql id="t0h9yn"
' AND 1=1--
```

```sql id="7v4t5v"
' AND 1=2--
```

Different page responses confirm injection.

---

## Time-Based Blind SQLi

The attacker forces the database to delay responses.

Example (PostgreSQL):

```sql id="m4lz1r"
'; SELECT pg_sleep(5)--
```

Example (MySQL):

```sql id="hwyc8p"
' AND SLEEP(5)--
```

If the response is delayed, the injection is successful.

---

## Error-Based Blind SQLi

Attackers trigger conditional database errors to infer information.

Example:

```sql id="3e8zh7"
' AND (SELECT CASE WHEN (1=1) THEN 1/0 ELSE 'a' END)--
```

---

## Out-of-Band (OAST) SQLi

The database interacts with an external server controlled by the attacker.

Used when:

* No visible output exists
* No timing differences exist

Commonly performed using:

* Burp Collaborator
* DNS callbacks
* HTTP callbacks

---

# 🔍 Identifying Blind SQL Injection

## Common Indicators

* Different content length
* Different response behavior
* Delayed responses
* Conditional redirects
* Subtle UI changes

---

# 🚀 Exploitation Methodology

## Step 1 — Test Basic Injection

```sql id="v5u7zw"
'
"
```

---

## Step 2 — Boolean Testing

```sql id="jlwmmd"
' AND 1=1--
' AND 1=2--
```

Observe response differences.

---

## Step 3 — Time Delay Testing

```sql id="8fxn3f"
' AND SLEEP(5)--
```

If the server delays the response, injection is confirmed.

---

## Step 4 — Extract Data Character by Character

Example:

```sql id="lzjlwm"
' AND SUBSTRING((SELECT password FROM users WHERE username='administrator'),1,1)='a'--
```

Attackers repeat this process to enumerate data.

---

# 💥 Example Scenario

Vulnerable query:

```sql id="jvlm3n"
SELECT * FROM products WHERE category = '$category';
```

Injected payload:

```sql id="d64tqf"
' AND 1=1--
```

Result:

```sql id="xjlwmk"
SELECT * FROM products WHERE category = '' AND 1=1--';
```

The injected condition modifies the query logic.

---

# ⚠ Impact

Blind SQL injection may lead to:

* Sensitive database extraction
* Authentication bypass
* Credential theft
* Administrative access
* Complete database compromise

Even without visible output, attackers can fully enumerate databases over time.

---

# 🛡 Prevention

## Recommended Mitigations

* Use prepared statements
* Implement parameterized queries
* Avoid dynamic SQL construction
* Sanitize user input
* Disable verbose database errors
* Use least-privileged database accounts
* Implement WAF protections

---

# 🧰 Tools Used

* Burp Suite
* Burp Repeater
* Burp Intruder
* Burp Collaborator
* SQLMap

---

# 📚 Key Learning

Blind SQL injection demonstrates that suppressing errors alone does not prevent exploitation.

Attackers can still extract sensitive data by observing application behavior, response timing, and conditional logic.

Understanding blind SQLi is essential for advanced web application penetration testing and bug bounty hunting.

---

# 🏷 Tags

`#BlindSQLi` `#SQLInjection` `#BurpSuite` `#OWASP` `#WebSecurity`
