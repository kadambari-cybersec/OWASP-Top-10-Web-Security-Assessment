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

## Phase 2 - Web Application Enumeration

Objective
To identify accessible web application components and potential attack surfaces for security testing.

Activities Performed

Accessed the DVWA web application through the HTTP service discovered during reconnaissance.
Authenticated using valid application credentials.
Navigated through the available DVWA modules.
Configured the application security level to Low for testing purposes.
Identified the SQL Injection module as a potential target for assessment.

# Evidence
![DVWA Login Successful](screenshots/02-dvwa-login-successful.png)
![Security Level Low](screenshots/03-security-low.png)

# OUTPUT
The SQL Injection module was successfully identified and prepared for exploitation testing in the next phase.

## Phase 3 - SQL Injection Exploitation
# Objective
To verify whether the User ID parameter was vulnerable to SQL Injection attacks.

Normal Application Behavior

The value 1 was supplied to the User ID parameter. The application returned a single record associated with the specified user.

# Evidence
![Normal Query](screenshots/04-normal-query.png)

## SQL Injection Payload
1' OR '1'='1

Exploitation Result
The payload altered the backend SQL query logic and caused the application to return all records from the users table, including multiple user accounts.

OUTPUT
First name: admin
Surname: admin

First name: Gordon
Surname: Brown First

name: Hack 
Surname: Me

First name: Pablo
Surname: Picasso

First name: Bob
Surname: Smith

Security Impact
Unauthorized disclosure of database records.
Exposure of sensitive user information.
Bypass of intended query restrictions.
# Evidence
![SQL Injection Successful](screenshots/05-sql-sucessful.png)

# OUTPUT
The SQL Injection vulnerability was successfully exploited, demonstrating that unsanitized user input could manipulate database queries and expose unauthorized information.

## Phase 4 - Mitigation& Remediation
# Objective
To evaluate the effectiveness of security control against the previously identified SQL Injection vulnerability.

# Activites performed
Changed the DVWA security level from Low to Medium.
Revisited the SQL Injection module.
Retested the previously successful SQL Injection payload.

Payload Tested
1' OR '1'='1

# Output
You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near '\' OR \'1\'=\'1'

 # Result
 At the Low security level, the payload successfully returned all records from the users table. After increasing the security level to Medium, the same payload no longer produced the previous result and generated a database error instead.

 # Evidence
 






