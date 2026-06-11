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
 ![Security Level Changed](screenshots/06-secuirty-changed.png)
![Mitigation Successful](screenshots/07-mitigation-successful.png)

# Security Impact
The mitigation reduced the effectiveness of the SQL Injection attack by introducing additional input handling controls. Although the application still displayed an error message, the attack no longer disclosed all user records.

# Outcome
The mitigation test demonstrated that additional security controls can significantly reduce the risk of SQL Injection attacks and improve the overall security posture of the application.

## conclusion
In this project, I successfully identified and exploited a SQL Injection vulnerability in DVWA and then tested mitigation techniques to reduce the risk. This assessment improved my understanding of web application security testing, vulnerability analysis, and secure coding practices.

## Module 2- Authentication
# Objective
The objective of this module was to evaluate the authentication mechanism implemented in DVWA and identify weaknesses related to user authentication and password security.

# Phase 1 - Authentication Enumeration
The DVWA Brute Force module was accessed to understand how the authentication process works. The login page contained username and password input fields and allowed users to submit credentials for authentication.
Different invalid credentials were tested to observe application behavior. The same error message was displayed for both invalid usernames and incorrect passwords, indicating that username enumeration was not observed during testing.

# Evidence
![Incorrect Login Authentication](screenshots/08-incorrect-login-authentication.png)

# outcome
The authentication mechanism was identified, and no username enumeration was observed.

# Phase 2 - Weak Password Testing
Common credentials were tested against the login form. The username admin and password password successfully authenticated and provided access to the protected area.

# Evidence
![Weak Password](screenshots/11-weak-password.png)

# Outcome
A weak password was accepted by the application, which could increase the risk of unauthorized access.

# Phase 3 - Authentication Bypass Testing
An authentication bypass attempt was performed using the payload:
admin' --
The application returned an SQL error and access was not granted.
 
 # Evidence
![Authentication Bypass Testing](screenshots/09-Authentication-bypass-testing.png)

# Outcome
The bypass attempt was unsuccessful, but the application exposed database error information.

# Phase 4 - Mitigation & Remediation
To improve authentication security, the following controls are recommended:
Use strong passwords.
Enable account lockout after multiple failed login attempts.
Implement CAPTCHA protection.
Enable Multi-Factor Authentication (MFA).
Avoid displaying database error messages.

# Outcome
These controls can help reduce the risk of brute-force attacks and unauthorized access.

## Conclusion
In this module, I tested the authentication functionality of DVWA and identified a weak password issue. I also performed authentication bypass testing and learned how authentication weaknesses can affect web application security.

## Module 3- Path Traversal(Sensitive File Disclosure Assessment)
# Objective

The objective of this module was to test whether the File Inclusion feature in DVWA could be abused to access files outside the web application directory.
 
 ## Phase 1 – Exploring the File Inclusion Module

I logged into DVWA and set the security level to Low. After opening the File Inclusion module, I noticed that the application loaded pages using a URL parameter. This suggested that user input was being used to determine which file should be displayed.

## Evidence
![Before Testing File](screenshots/12-before-testing-file.png)

## Outcome

The application appeared to be using user-controlled input for file loading, making it a possible target for Path Traversal testing.

## Phase 2 – Initial Testing
I started testing directory traversal sequences to see if files outside the application folder could be accessed.

Payload Used
../../../etc/passwd

Result
The application returned an error and did not display the file contents.
 
 ## Evidence
 ![Exploit Path Traversal](screenshots/13-exploite-path-traversal.png)

## Outcome
The attempt failed because the traversal depth was not enough to reach the target file. However, the application still tried to process the supplied path, indicating that file inclusion was occurring.

## Phase 3 – Successful Exploitation
I increased the traversal depth and tested again.

Payload Used
../../../../../../etc/passwd

Result
The application successfully displayed the contents of the Linux system file:

## Evidence
![Attack Successful](screenshots/15-attack-succesful.png)
aylo
/etc/passwd
Sample Output
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/bin/sh
www-data:x:33:33:www-data:/var/www:/bin/sh
msfadmin:x:1000:1000:msfadmin:/home/msfadmin:/bin/bash
Security Impact
Access to sensitive system files.
Disclosure of local user account information.
Exposure of information that could help an attacker perform further attacks.

## Outcome
The Path Traversal vulnerability was successfully exploited, proving that files outside the intended web directory could be accessed.

## Phase 4 – Mitigation & Recommendations
To reduce the risk of Path Traversal vulnerabilities, the following controls should be implemented:
Validate and sanitize user input.
Block traversal sequences such as ../.
Use an allowlist of approved files.
Avoid displaying detailed error messages.
Apply the principle of least privilege.
 
 ## Outcome
These controls can help prevent unauthorized file access and reduce information disclosure risks.

# Conclusion

In this module, I successfully identified and exploited a Path Traversal vulnerability in DVWA. An initial attempt failed due to insufficient traversal depth, but after modifying the payload, I was able to access the /etc/passwd file and view sensitive system information. This project improved my understanding of file inclusion vulnerabilities, directory traversal techniques, and secure input validation practices.

## Module 4 - Command Injection
# objective
The application takes the user supplied input details and passes it directly to the operating system without any proper validiation or sanitixation.

## Phase 1 - Reconnaissance
# objective
To identify the target and web services

payload :- namp -sV 192.168.56.103 

## Result -
22/tcp open ssh
80/tcp open http

# Evidence
![Command Injection Identification Phase](screenshots/16-command-injection-identification-phase.png)

## outcome
The target system was successfully enumerated. Open services including HTTP (Port 80) and SSH (Port 22) were identified and prepared for further assessment.

## phase 2 - web application enumeration

# objective
Identify the Command Injection functionality within DVWA.

Activity performed 

Accessed DVWA through the web interface.
Authenticated using valid credentials.
Configured DVWA security level to Low.
Navigated to the Command Injection module.
Observed an input field designed to accept IP addresses for ping operations.

# Evidence 
![DVWA Login Successful](screenshots/dvwa-login-successful.png)
![Security Low](screenshots/security-low.png)

## outcome
The Command Injection module was successfully identified and prepared for testing.

## phase 3 - Command Injection Exploitation
Objective
To determine whether user-supplied input could execute unauthorized operating system commands.

Normal function testing 

Input
127.0.0.1

Output
Successful ping response.

## evidence 
![Normal Func Testing](screenshots/17-normal-func-testing.png)

## Test 1 – whoami

Payload
127.0.0.1; whoami

Result
The application returned the username "www-data".

# Evidence 
![CI Semicolon Whoami](screenshots/20-CI-%20semicolon-whoami.png)

Analysis
The additional command was executed by the operating system, confirming the presence of Command Injection.

Test 2 – id

Payload
127.0.0.1 && id

Result
uid=33(www-data)
gid=33(www-data)
groups=33(www-data)

##  Evidence 
![CI && ID](screenshots/19-CI-%26%26-id.png)

Analysis
The application executed the id command and disclosed user and group information associated with the web

Test 3 – hostname

Payload
127.0.0.1 | hostname

## Evidence 
![CI Pipe Hostname](screenshots/18-CI-pipe-hostname.png)

Analysis

Successful execution of the hostname command allowed disclosure of system identification information. This confirmed that the application was vulnerable to Command Injection and could expose details about the underlying host environment..

Test 5 – uname -a

Payload
127.0.0.1; uname -a

Analysis
The command returned operating system and kernel details, demonstrating system information disclosure.

## Phase 4 – Mitigation & Remediation
 
 # Objective
Evaluate the effectiveness of security controls against the identified vulnerability.

Activities Performed

Changed DVWA security level from Low to Medium.
Retested previously successful payloads.
Observed application behaviour after mitigation.

Payload Tested
127.0.0.1; whoami

Result
Describe exactly what happened in your lab.

Security Impact
The mitigation reduced the ability of attackers to execute arbitrary operating system commands through user-controlled input.

## Outcome
The implemented controls reduced the effectiveness of the Command Injection attack and improved application.

# conclusion
This assessment successfully demonstrated a Command Injection vulnerability within DVWA. Multiple operating system commands were executed through unsanitized user input, resulting in system information disclosure and user enumeration. Mitigation testing showed that additional security controls reduced the effectiveness of the attack. This project improved my understanding of web application security testing, operating system interaction, command execution risks, and secure input handling practices.











