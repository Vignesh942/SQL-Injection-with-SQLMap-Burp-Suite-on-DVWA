# SQL-Injection-with-SQLMap-Burp-Suite-on-DVWA



## 📌 Overview
This project demonstrates how to perform **SQL Injection** using **SQLMap** and **Burp Suite** on **Damn Vulnerable Web Application (DVWA)** to enumerate and exploit databases.

---
## 🛠 Requirements
- **Kali Linux** (or any OS with Burp Suite & SQLMap installed)
- **Burp Suite Community Edition**
- **SQLMap**
- **DVWA (Damn Vulnerable Web App)** running on **Metasploitable 2**

---
## 🔥 Steps to Exploit SQL Injection

### 1️⃣ Capture the Request using Burp Suite
1. Open **Burp Suite** and enable **Intercept** in the **Proxy** tab.
2. In your browser, navigate to `http://<DVWA-IP>/dvwa/vulnerabilities/sqli/`.
3. Enter any value in the input field and submit the form.
4. Burp Suite will capture the **HTTP Request**.
5. Right-click the request → **Save item** as `request.txt`.

### 2️⃣ Use SQLMap to Test SQL Injection
Run the following command in your terminal:
```sh
sqlmap -r request.txt --dbs --batch
```
- `-r request.txt` → Uses the saved HTTP request.
- `--dbs` → Lists available databases.
- `--batch` → Auto confirms prompts.

### 3️⃣ Enumerate Tables & Columns
Once databases are listed, pick `dvwa` and enumerate tables:
```sh
sqlmap -r request.txt -D dvwa --tables
```
Then, retrieve columns of the `users` table:
```sh
sqlmap -r request.txt -D dvwa -T users --columns
```

### 4️⃣ Dump User Credentials
```sh
sqlmap -r request.txt -D dvwa -T users --dump
```
This extracts stored usernames and passwords from DVWA.

---
## 🎯 Expected Output
- Lists available **databases**.
- Extracts **tables and columns** from `dvwa`.
- Dumps **user credentials** stored in the database.

---
## ⚠️ Legal Disclaimer
This project is for **educational purposes only**. Do not test SQL injection on unauthorized systems. Always obtain permission before conducting security tests.

---
## 📷 Screenshots



![Screenshot 2025-02-11 190857](https://github.com/user-attachments/assets/a4f9863c-5948-4ebc-a75b-9d8c1f54f671)
![Screenshot 2025-02-11 191207](https://github.com/user-attachments/assets/9002e8b5-2b3d-4ce4-8d89-5e9bd4bf3411)
![Screenshot 2025-02-11 192130](https://github.com/user-attachments/assets/9cf8dc65-305e-479b-91ef-ca8537e96089)


For more References vist:
- [SQLMap Documentation](https://sqlmap.org/)
- [Burp Suite Official Site](https://portswigger.net/burp)

