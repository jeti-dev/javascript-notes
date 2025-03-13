---
title: Security
layout: default
---

## OWASP Top 10 Security Issues

| Rank   | Issue                                          | Description                                                                                           | Example                                                                                                  |
| ------ | ---------------------------------------------- | ----------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------- |
| **1**  | **Broken Access Control**                      | Improper enforcement of permissions, allowing users to access unauthorized data or actions.           | Accessing `/admin` without proper authentication or modifying another user's data via a direct API call. |
| **2**  | **Cryptographic Failures**                     | Sensitive data is exposed due to weak or improper cryptographic practices.                            | Storing passwords without hashing, using outdated encryption like MD5.                                   |
| **3**  | **Injection**                                  | Untrusted data is sent to an interpreter (e.g., SQL, OS commands), allowing malicious code execution. | `input = "'; DROP TABLE users; --"` leading to SQL injection.                                            |
| **4**  | **Insecure Design**                            | Design flaws that create vulnerabilities before coding begins.                                        | Lack of proper threat modeling or secure architecture patterns.                                          |
| **5**  | **Security Misconfiguration**                  | Improperly configured security settings or missing security headers.                                  | Exposing stack traces or allowing directory listing on a production server.                              |
| **6**  | **Vulnerable and Outdated Components**         | Use of libraries, frameworks, or components with known vulnerabilities.                               | Using an outdated `express.js` version that has a known XSS vulnerability.                               |
| **7**  | **Identification and Authentication Failures** | Weak authentication or session handling mechanisms.                                                   | Allowing login without a password or using HTTP instead of HTTPS.                                        |
| **8**  | **Software and Data Integrity Failures**       | Code or data are improperly verified or tampered with.                                                | Loading JavaScript libraries from an external, untrusted CDN.                                            |
| **9**  | **Security Logging and Monitoring Failures**   | Lack of proper logging or monitoring to detect and respond to breaches.                               | No logs for login attempts or missing alerts for failed access attempts.                                 |
| **10** | **Server-Side Request Forgery (SSRF)**         | Attacker forces a server to make requests to internal or external systems.                            | Making an internal HTTP request via an open endpoint like `/api/fetch?url=http://localhost/admin`.       |

## Other Known Vulnerabilities

| Vulnerability                             | Description                                                                                            | Example                                                                              |
| ----------------------------------------- | ------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------ |
| **Cross-Site Scripting (XSS)**            | Injection of malicious scripts into web pages viewed by other users.                                   | `<script>alert('XSS')</script>` in a user comment field.                             |
| **Cross-Site Request Forgery (CSRF)**     | Tricking a user into making an unintended request to another site where they are authenticated.        | A hidden form submission that transfers funds without user consent.                  |
| **Race Conditions**                       | Two or more processes attempt to modify shared data simultaneously, leading to unpredictable behavior. | A bank withdrawal processed twice due to race conditions in the transaction code.    |
| **Directory Traversal**                   | Improper input sanitization allowing access to files outside the intended directory.                   | Accessing `/etc/passwd` by sending `/../../etc/passwd` in a URL.                     |
| **Clickjacking**                          | Embedding a malicious overlay on a legitimate site to trick users into clicking.                       | An invisible iframe over a login button to steal credentials.                        |
| **Open Redirect**                         | Redirecting users to untrusted sites due to improper validation of URL parameters.                     | `example.com/redirect?url=http://malicious.com` leading to phishing.                 |
| **Insecure Deserialization**              | Exploiting unsafe deserialization to execute arbitrary code.                                           | Sending a crafted serialized object to exploit a JavaScript `eval()` function.       |
| **Mass Assignment**                       | Allowing attackers to modify object properties that should not be exposed.                             | `req.body = { "role": "admin" }` assigning admin rights directly from user input.    |
| **Server-Side Template Injection (SSTI)** | Injecting malicious template code into a server-side template engine.                                  | `{{7*7}}` in a template engine returning `49`.                                       |
| **HTTP Response Splitting**               | Injecting headers into an HTTP response to manipulate browser behavior.                                | `GET /index.php?name=%0D%0ASet-Cookie%3A+admin%3Dtrue` leading to session hijacking. |
| **Unvalidated Redirects and Forwards**    | Redirecting users to malicious sites without validating the target URL.                                | `example.com/redirect?to=http://malicious.com`.                                      |
| **Session Fixation**                      | Setting a user's session ID before login, allowing session hijacking.                                  | Pre-setting `PHPSESSID` before login and reusing it post-login.                      |
