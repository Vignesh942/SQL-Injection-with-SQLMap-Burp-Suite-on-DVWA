# SQL Injection Testing on DVWA using SQLMap & Burp Suite

Author: Vignesh G
Date: February 2025
Tools Used: Burp Suite, SQLMap, DVWA (Metasploitable 2)

## Objective   
To identify and exploit SQL Injection vulnerabilities in the Damn Vulnerable Web Application (DVWA) using SQLMap and Burp Suite, demonstrating how attackers can extract sensitive database information.

 ## Environment Setup
Attacker Machine: Kali Linux (Burp Suite + SQLMap installed)
Target Machine: Metasploitable 2 (DVWA running on Apache/PHP/MySQL)
Network: Host-only or NAT configuration for safe testing


## Methodology
Reconnaissance
DVWA was accessed at http://<target-ip>/dvwa/vulnerabilities/sqli/.
A test input was submitted into the User ID field to capture the request in Burp Suite.
Interception
Burp Suite Proxy was used to intercept the HTTP request.
The captured request was saved locally as request.txt.

## Exploitation using SQLMap
The saved request was passed to SQLMap for testing SQL injection.
Step 1: Database Enumeration
sqlmap -r request.txt --dbs --batch

Result: SQLMap successfully detected a SQL Injection vulnerability and listed the available databases.

Step 2: Table Enumeration
sqlmap -r request.txt -D dvwa --tables

Result: Tables such as users, guestbook, and visitors were found.
Step 3: Column Enumeration
sqlmap -r request.txt -D dvwa -T users --columns

Result: Columns like user_id, user, password were identified.
Step 4: Dumping Data
sqlmap -r request.txt -D dvwa -T users --dump

Result: Extracted usernames and password hashes from the users table.


## Observations
The parameter id in the HTTP request was found to be vulnerable to SQL Injection.
SQLMap was able to automate data extraction without authentication bypass.
Passwords were stored as hashes, showing partial security awareness but poor input sanitization


## Mitigation Recommendations
Use parameterized queries or prepared statements in database queries.
Sanitize and validate all user input before execution.
Implement a Web Application Firewall (WAF) to block injection attempts.
Regularly perform vulnerability scans and code reviews.


## Conclusion
The test successfully demonstrated how SQL Injection can be exploited using automated tools like SQLMap.
 This highlights the need for secure coding practices and regular vulnerability assessments in web applications.

## Demo



<img width="1866" height="935" alt="Screenshot 2025-10-30 163331" src="https://github.com/user-attachments/assets/af0f9ba2-4b7f-45df-91c3-1414d89cfa34" />

<img width="431" height="126" alt="Screenshot 2025-10-30 163451" src="https://github.com/user-attachments/assets/cc1b4492-def7-43b5-9dd3-ad740171b4fc" />

<img width="1910" height="251" alt="Screenshot 2025-10-30 163554" src="https://github.com/user-attachments/assets/30f21019-6f3d-43b1-b22d-de4b472e2587" />

Download PDf Here:

[Proof of Concept_ DDoS Simulation and Analysis.pdf](https://github.com/user-attachments/files/23232753/Proof.of.Concept_.DDoS.Simulation.and.Analysis.pdf)






