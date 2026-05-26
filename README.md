# owasp-top10-web-security-assessment
## Module 1:-SQL-Injection
A controlled web application security assessment project demonstrating SQL Injection exploitation, HTTP request interception, authentication bypass techniques, Python-based automation, and defensive mitigation strategies using Kali Linux, Metasploitable2, DVWA, Nmap, and Burp Suite.

## Lab setup
component & purpose
 Kali Linux | Attacker machine used for security testing 
 Metasploitable2 | Vulnerable target machine 
 DVWA | Vulnerable web application for SQL Injection testing 
 Burp Suite | HTTP request interception and analysis 
 Nmap | Network service enumeration and scanning 

## Phase 1 - Reconnaissance
Nmap service enumeration identified exposed services on the target machine.

Command Used
Bash:-nmap -sV 192.168.29.130. Scan Result

Detected services:
Port 22 (SSH)
Port 80 (HTTP)

## Evidence
![Nmap Scan](screenshots/01-nmap-scan.png)

## Phase 2 - Web Application Exploitation
Burp Suite was used to intercept HTTP requests and identify SQL Injection vulnerabilities.

SQL Injection Payload
1' OR '1'='1
Exploitation Result
