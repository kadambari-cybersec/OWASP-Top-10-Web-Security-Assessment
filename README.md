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
The DVWA application was accessed through the HTTP service identified during reconnaissance. The application was reviewed to identify user input fields and potential attack surfaces.

# Activities Performed
Accessed the DVWA web interface.
Authenticated using valid credentials.
Configured the security level to Low.
Identified the SQL Injection module for further testing.
Observed that the User ID parameter accepted unsanitized user input.

# Evidence
![DVWA Login Successful](screenshots/02-dvwa-login.png)







