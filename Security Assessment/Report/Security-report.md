# Security Assessment Report – OWASP Juice Shop

## 1. Introduction

This report presents a security assessment conducted on the OWASP Juice Shop application. The objective is to identify vulnerabilities, analyze their impact, and recommend fixes.

---

## 2. Methodology

- Application tested locally
- Manually tested
- Tools used: Burp Suite, OWASP ZAP, Browser DevTools
- Evidence collected using screenshots

---

## 3. Findings

### 3.1 SQL Injection

#### Description

SQL Injection is a vulnerability that allows an attacker to manipulate database queries by injecting malicious input into application fields.

---

#### Steps to SQL Inject

1. Go to the login page of the application
2. Enter the following payload in the email field:
   ' OR 1=1 --
3. Enter any password
4. Click on the login button

---
  
#### Proof of Concept

**Login Payload**
![SQL Payload](../Evidence/SQLi/login%20playload.png)

**Successful Login**
![Login Success](../Evidence/SQLi/admin%20account.png)

---

#### Impact

An attacker can bypass authentication and gain unauthorized access to user accounts. This may lead to data theft, privilege escalation, and full database compromise.

---

#### Severity

High

---

#### Mitigation

- Use parameterized queries
- Validate user inputs
- Implement proper error handling



---

### 3.2 Cross-Site Scripting (XSS)

#### Description

Cross-Site Scripting (XSS) is a vulnerability that allows attackers to inject malicious scripts into web pages viewed by other users.

---

#### Steps to Reproduce

1. Navigate to the search bar in the application
2. Enter the following payload:

   <iframe src="javascript:alert('XSS')"></iframe> 

3. Press Enter

---

#### Proof of Concept

**Payload Input**
![XSS Payload](../Evidence/XSS/entering%20payload.png)

**Alert Execution**
![XSS Alert](../Evidence/XSS/payload%20works.png)

---

#### Impact

Attackers can execute malicious scripts in users' browsers, steal session cookies, and perform actions on behalf of users.

---

#### Severity

High

---

#### Mitigation

* Validate user input
* Encode output data
* Implement Content Security Policy (CSP)



---

### 3.3 Insecure Direct Object Reference (IDOR)

#### Description

IDOR is a vulnerability that occurs when an application allows access to objects based on user-supplied input without proper authorization checks.

---

#### Steps to Reproduce

1. Intercept a request using Burp Suite
2. Identify a parameter such as:
   id=1
3. Modify the value to:
   id=2
4. Forward the request

---

#### Proof of Concept

**Original Request**
![Original](../Evidence/IDOR/original%20request.png)

**Modified Request**
![Modified](../Evidence/IDOR/modified%20request.png)

**Response Showing Other User Data**
![Response](../Evidence/IDOR/modified%20requests.png)

---

#### Impact

An attacker can access other user's sensitive data. It can lead to privacy violations and data leakage.

---

#### Severity

High

---

#### Mitigation

* Implement proper authorization checks
* Validate user permissions on every request
* Use indirect object references



---

### 3.4 Weak Authentication (Default Credentials)

#### Description

The application allows login using weak or default credentials, making it easy for attackers to gain unauthorized access.

---

#### Steps to Reproduce

1. Navigate to login page
2. Enter:
   Email: [admin@juice-sh.op]
   Password: admin123
3. Click login

---

#### Proof of Concept

**Login Attempt**
![Login](../Evidence/Authentication%20Issue/login%20attempt.png)

**Successful Login**
![Login](../Evidence/Authentication%20Issue/successful%20login.png)

**Admin Access**
![Admin Access](../Evidence/Authentication%20Issue/admin%20dashboard.png)

---

#### Impact

Attackers can gain administrative access, leading to full system compromise, data modification, and data theft.

---

#### Severity

High

---

#### Mitigation

* Enforce strong password policies
* Disable default credentials
* Implement multi-factor authentication (MFA)
* Lock accounts after multiple failed attempts



---

### 3.5 Security Misconfiguration (Directory Listing Enabled)

#### Description

The application exposes the /ftp directory, allowing users to view and access sensitive files without authentication.

---

#### Steps to Reproduce

1. Scan the web app using OWASP ZAP (use ajax spider)
3. Navigate to:
   /ftp
2. Observe the list of files
3. Open any file directly

---

#### Proof of Concept

**Directory Listing**
![Directory](../Evidence/Security%20Misconfig/scanning%20using%20ZAP.png)

**Directory Listing**
![Directory](../Evidence/Security%20Misconfig/access%20for%20sensitive%20files.png)

**Sensitive File Access**
![File Access](../Evidence/Security%20Misconfig/sensitive%20file.png)

---

#### Impact

Sensitive information may be exposed to attackers, which can be used for further exploitation.

---

#### Severity

High

---

#### Mitigation

* Disable directory listing
* Restrict access to sensitive directories
* Implement proper access controls



---

## 4. Risk Analysis

The identified vulnerabilities causes significant risks to the application. SQL Injection and Authentication issues can lead to full system compromise. XSS can affect users by executing malicious scripts. IDOR exposes sensitive user data and security misconfiguration allows unauthorized access to confidential files.

Overall, the application has multiple high-risk vulnerabilities that could be exploited by attackers.



---

## 5. Conclusion

This assessment identified several critical vulnerabilities in the OWASP Juice Shop application. These issues highlight the importance of secure coding practices, proper authentication mechanisms, and correct system configuration.

Implementing the recommended fixes will significantly improve the security posture of the application.
