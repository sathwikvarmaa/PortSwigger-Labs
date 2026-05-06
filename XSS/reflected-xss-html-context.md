Reflected XSS into HTML Context
📌 Lab Description

This lab contains a reflected Cross-Site Scripting (XSS) vulnerability in the search functionality.
User input is reflected directly inside the HTML response without proper sanitization or encoding.

The goal of the lab is to execute JavaScript in the victim's browser.

🧠 Vulnerability Type
Reflected Cross-Site Scripting (XSS)
HTML Context Injection
📖 What is Reflected XSS?

Reflected XSS occurs when user-controlled input is immediately returned by the web application in the HTTP response without proper validation or output encoding.

The malicious payload executes when the victim visits a crafted URL.

🔍 Identifying the Vulnerability

The search parameter was reflected directly inside the HTML page.

Example request:

GET /?search=test HTTP/1.1
Host: vulnerable-website.com

The response contained:

<h1>0 search results for 'test'</h1>

Since the application reflected the input without sanitization, HTML/JavaScript payloads could be injected.

💥 Payload Used
<script>alert(1)</script>

Encoded version:

%3Cscript%3Ealert(1)%3C/script%3E
🚀 Exploitation Steps
Open the vulnerable search functionality.
Intercept the request using Burp Suite.
Modify the search parameter with an XSS payload.
Forward the request.
Observe JavaScript execution in the browser.

Example:

GET /?search=<script>alert(1)</script> HTTP/1.1
✅ Successful Exploit

The application executed arbitrary JavaScript in the browser, confirming the presence of a reflected XSS vulnerability.

⚠ Impact

An attacker may:

Steal session cookies
Perform actions on behalf of users
Deface web pages
Redirect users to malicious sites
Capture sensitive information
Deliver phishing attacks
🛡 Prevention
Recommended Mitigations
Perform proper output encoding
Sanitize user-controlled input
Use Content Security Policy (CSP)
Implement secure templating frameworks
Validate input on both client and server side
🧰 Tools Used
Burp Suite
Browser Developer Tools
PortSwigger Web Security Academy
📚 Key Learning

This lab demonstrates how reflecting unsanitized user input inside an HTML response can lead to JavaScript execution and client-side compromise.

Understanding input reflection contexts is essential for identifying and exploiting XSS vulnerabilities effectively.

🏷 Tags

#XSS #ReflectedXSS #WebSecurity #BurpSuite #PortSwigger
