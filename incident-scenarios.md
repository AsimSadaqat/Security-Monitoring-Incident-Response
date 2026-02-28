# 🚨 Incident Scenarios & Response Steps

## 📌 Overview

This document outlines simulated security incidents detected during log monitoring.  
Each scenario includes detection method, severity classification, potential impact, and structured response steps.

---

# 1️⃣ Scenario: SQL Injection Attempt

## Incident Description
Multiple HTTP requests containing encoded SQL manipulation patterns were detected in Apache access logs.

Observed Pattern:
%27%20OR%20%271%27=%271  
(Decoded: ' OR '1'='1)

## Detection Source
/var/log/apache2/access.log

## Severity
High

## Potential Impact
- Authentication bypass  
- Unauthorized database access  
- Data leakage or modification  

## Response Steps
1. Review full access logs for repeated attempts  
2. Identify and monitor source IP address  
3. Block IP if malicious activity persists  
4. Validate input sanitization in the application  
5. Document incident details  
6. Close case after verification  

---

# 2️⃣ Scenario: Directory Traversal Attempt

## Incident Description
A request attempting to access a sensitive system file (`/etc/passwd`) was observed.

## Detection Source
/var/log/apache2/access.log

## Severity
High

## Potential Impact
- Exposure of system configuration files  
- Information disclosure vulnerability exploitation  

## Response Steps
1. Confirm whether file access was successful  
2. Block source IP if suspicious behavior continues  
3. Review web server file permissions  
4. Ensure directory traversal protections are enabled  
5. Document findings  
6. Close incident after validation  

---

# 3️⃣ Scenario: SSH Brute Force Attempt

## Incident Description
Multiple failed SSH login attempts were detected from the same IP address within a short time period.

Example Log Entry:
Failed password for invalid user fakeuser from 127.0.0.1

## Detection Source
journalctl -u ssh

## Severity
Medium  
(Escalates to High if login succeeds)

## Potential Impact
- Unauthorized remote shell access  
- Full system compromise  

## Response Steps
1. Identify source IP address  
2. Temporarily block IP if threshold exceeded  
3. Review logs for successful authentication attempts  
4. Enforce strong password policies  
5. Enable SSH key-based authentication  
6. Document the incident  
7. Monitor system for further suspicious activity  

---

# 📊 Severity Summary

| Incident Type         | Severity | Impact Level                    |
|-----------------------|----------|--------------------------------|
| SQL Injection         | High     | Web Application Compromise     |
| Directory Traversal   | High     | Sensitive File Exposure        |
| SSH Brute Force       | Medium   | Authentication Attack          |

---

# ✅ Conclusion

These simulated scenarios demonstrate practical SOC operations including detection, classification, response execution, and documentation.  
Each incident was evaluated based on impact, frequency, and attack intent.