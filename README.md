# owasp-top10-web-security-assessment
# Module 1:-SQL-Injection
A controlled web application security assessment project demonstrating SQL Injection exploitation, authenticated access validation, post-exploitation analysis, and defensive mitigation techniques using Kali Linux, Metasploitable2, DVWA, Nmap, and Burp Suite.

## Phase 1 - Reconnaissance
Nmap service enumeration identified exposed services on the target machine.

Command Used
nmap -sV 192.168.29.130. Scan Result

Detected services:
Port 22 (SSH)
Port 80 (HTTP)

## Phase 2 - Web Application Exploitation
Burp Suite was used to intercept HTTP requests and identify SQL Injection vulnerabilities.

SQL Injection Payload
1' OR '1'='1
Exploitation Result
