# 🛡️ E-commerce Security Internship – Phase 2: Risk and Compliance

This document summarizes the findings and risk analysis of a simulated e-commerce environment running OpenCart over Apache, as part of a self-study cybersecurity internship.

---

## 🔍 Step 1: Asset Identification

| Asset                   | Description                                        | Notes                                                               |
| ----------------------- | -------------------------------------------------- | ------------------------------------------------------------------- |
| Admin Login Page        | Backend login interface (`/admin`)                | Accessible via HTTP                                                 |
| Admin Username          | Default user `admin`                              | Should be changed                                                   |
| Login Protection        | No CAPTCHA or rate limit                          | Brute-force vulnerability                                           |
| OpenCart Database       | MySQL database holding users, orders, products    | Named `opencart`                                                    |
| Database User           | MySQL user accessing DB                           | `opencartuser@localhost`                                            |
| DB Credentials Location | Found in config files                             | `/var/www/html/config.php`, `/admin/config.php`                     |

> 🚫 Never expose config.php or admin config files publicly!

---

## 🧨 Step 2: Threats & Vulnerabilities

| Asset                      | Threat                            | Description                                                                 |
| -------------------------- | --------------------------------- | --------------------------------------------------------------------------- |
| Customer Database          | SQL Injection                     | Extract sensitive user data                                                 |
| Admin Panel Login          | Brute Force / Credential Stuffing | Repeated login attempts may bypass login                                    |
| Payment Page               | Formjacking / MITM                | Card data can be intercepted                                                |
| Apache Web Server          | RCE / Directory Listing           | Exploits can disrupt the service or expose sensitive files                  |
| Configuration Files        | Info Disclosure / LFI             | Database credentials leak                                                   |
| Uploads Folder             | RCE via Malicious File Upload     | Attackers can upload shells                                                 |
| Session Management System  | Session Hijacking                 | Predict or steal session tokens                                             |
| Email / Notification System| Phishing                          | Fake emails to users                                                        |
| Inventory System           | Tampering                         | Change prices, manipulate stock                                             |
| Logs and Audit Trails      | Tampering                         | Remove attack evidence                                                      |

---

## 🧪 Step 3: Risk Assessment

| Asset                | Threat                            | Likelihood | Impact | Risk Level |
|---------------------|------------------------------------|------------|--------|------------|
| Customer Database    | SQL Injection                     | High       | High   | Critical   |
| Admin Panel Login    | Brute Force                       | Medium     | High   | High       |
| Payment Page         | Formjacking / MITM                | Medium     | High   | High       |
| Apache Web Server    | RCE / Directory Access            | Medium     | High   | High       |
| Configuration Files  | Info Disclosure                   | Medium     | Critical| Critical   |
| Uploads Folder       | Malicious Upload / RCE            | Medium     | High   | High       |
| Session System       | Session Hijacking                 | Medium     | Medium | Medium     |
| Email System         | Phishing / Spoofing               | Low        | Medium | Low        |
| Inventory System     | Tampering                         | Low        | Medium | Low        |
| Logs & Audit Trails  | Log Deletion                      | Medium     | High   | High       |

---

## 📋 Step 4: Compliance Mapping

| Threat               | Required Control             | Framework       |
|----------------------|------------------------------|------------------|
| SQL Injection        | Input Validation             | PCI-DSS          |
| Unencrypted Data     | Data-at-Rest Encryption      | GDPR             |
| Unauthorized Access  | Role-Based Access Control    | ISO 27001        |
| Session Hijacking    | Secure Session Management    | OWASP ASVS       |
| Default Credentials  | Credential Hardening         | NIST SP 800-53   |
| Config Exposure      | Restrict File Permissions    | OWASP Top 10     |
| File Upload Exploits | File Type & Size Validation  | OWASP ASVS       |
| Email Spoofing       | SPF, DKIM, DMARC             | ISO 27001        |
| Lack of Logs         | Logging & Monitoring         | PCI-DSS          |

---

## 🔬 Web Vulnerability Assessment – Nikto Results

Target: `http://127.0.0.1/opencart`

| Risk # | Risk / Misconfiguration                                 | Description                                                  | Risk Level | Recommendation                                                             |
|--------|----------------------------------------------------------|--------------------------------------------------------------|------------|------------------------------------------------------------------------------|
| 1️⃣     | Cookies lack `HttpOnly` flag                             | Exposed to XSS attacks via JS                                | Medium     | Add `HttpOnly` to cookies                                                   |
| 2️⃣     | `Access-Control-Allow-Origin: *`                         | Open to Cross-Origin attacks                                 | Medium     | Restrict allowed origins                                                    |
| 3️⃣     | Missing `X-Frame-Options` header                         | Clickjacking risk                                            | Medium     | Add `SAMEORIGIN` or `DENY`                                                  |
| 4️⃣     | No `X-Content-Type-Options` header                       | MIME sniffing possible                                       | Low        | Add `nosniff` header                                                        |
| 5️⃣     | Sensitive paths in `robots.txt`                          | Exposes directories                                          | Info       | Review disallowed paths                                                     |
| 6️⃣     | Apache 2.4.52 is outdated                                | Known critical CVEs                                          | High       | Upgrade to 2.4.60+ or patch                                                 |
| 7️⃣     | Config files publicly accessible                         | Exposes DB credentials                                       | High       | Restrict file access via `.htaccess` or server config                       |
| 8️⃣     | Directory Indexing on `/system/` and `/image/`          | Allows listing files                                         | Medium     | Disable indexing                                                            |
| 9️⃣     | PHP backdoor file managers found                         | Indicates prior compromise                                   | Critical   | Remove immediately and scan for persistence                                |
| 🔟     | Shell and command injection files (`login.cgi`, etc.)     | Allows remote code execution                                 | Critical   | Remove and analyze for exploitation                                         |

---

## 📁 robots.txt Review

No critical exposure detected, but avoid adding paths like `/admin/`.

```txt
Disallow: /admin/
Disallow: /*?page=$
Disallow: /*&page=$
Disallow: /*?sort=
Disallow: /*?filter_name=
...
```

---

✅ **End of Phase 2: Risk and Compliance**