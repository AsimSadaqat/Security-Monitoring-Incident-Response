# 🔍 Detection Logic & Explanation

This document defines the detection rules created for monitoring web and system activity in the simulated SOC environment.

---

## 1️⃣ SQL Injection Detection Rule

### Trigger Condition
Alert if an HTTP request contains suspicious SQL manipulation patterns such as:

- `' OR`
- `--`
- `UNION SELECT`
- Encoded equivalents like:
  - `%27` (')
  - `%20` (space)

### Log Source
`/var/log/apache2/access.log`

### Why This Matters
SQL Injection is a common attack technique used to manipulate backend database queries.  
If successful, it may allow attackers to:

- Bypass authentication
- Extract sensitive database records
- Modify or delete data

### Severity
**High**

### Possible False Positives
- Application developer testing input validation
- Security testing in a controlled environment

---

## 2️⃣ Directory Traversal Detection Rule

### Trigger Condition
Alert if HTTP request contains:

- `../`
- `../../`
- Attempts to access sensitive system files such as:
  - `/etc/passwd`
  - `/etc/shadow`

### Log Source
`/var/log/apache2/access.log`

### Why This Matters
Directory traversal attacks attempt to access files outside the web root directory.  
If successful, attackers may gain access to sensitive system configuration files.

### Severity
**High**

### Possible False Positives
- Rare in production environments
- Misconfigured internal testing scripts

---

## 3️⃣ SSH Brute Force Detection Rule

### Trigger Condition
Alert if:

- More than 5 failed SSH login attempts
- From the same IP address
- Within 1 minute

### Log Source
`journalctl -u ssh`

### Why This Matters
Repeated failed login attempts indicate a potential brute-force password attack.  
If successful, an attacker may gain remote shell access and full system control.

### Severity
**Medium**

### Escalation Logic
If a successful login occurs after multiple failed attempts → Escalate to **High Severity**

### Possible False Positives
- User entering incorrect password multiple times
- Misconfigured automation script

---

# 📊 Detection Strategy Summary

| Rule | Log Source | Severity | Attack Type |
|------|------------|----------|------------|
| SQL Injection | Apache Access Log | High | Web Application Attack |
| Directory Traversal | Apache Access Log | High | File Access Exploit |
| SSH Brute Force | SSH Journal Logs | Medium | Authentication Attack |

---

# ✅ Conclusion

The detection rules implemented in this project simulate real-world SOC monitoring techniques.  
Each rule is designed to identify suspicious behavior patterns based on log analysis and defined severity thresholds.