Tags: #enumeration #mysql #database #pentesting  
Topic: Network Service Enumeration

---

#  Quick Concept

MySQL = database used by most websites.

- Default Port → **TCP 3306**
    
- Default User → `root`
    
- Default Password → Often blank or weak
    

 Main Goal:

- Empty root password
    
- Weak credentials
    
- Reused credentials
    

---

#  What You Can Find

If access is gained:

- Usernames
    
- Password hashes (or plain text)
    
- Emails
    
- Admin credentials
    
- API keys
    
- Application configs
    

Databases often contain EVERYTHING.

---

#  Attack Flow

Scan → Try Login → Explore DB → Dump Data → Crack/Reuse

---

# 1️ Scan Port 3306

sudo nmap -sV -sC -p3306 --script mysql* <TARGET_IP>

Look for:

- `mysql-empty-password`
    
- `mysql-info` (version)
    
- `mysql-enum` (usernames)
    
- `mysql-brute` (valid creds)
    

 Always manually verify results.

---

# 2️ Try Root Login

No password:

mysql -u root -h <TARGET_IP>

With password:

mysql -u root -pPASSWORD -h <TARGET_IP>

 No space after `-p`

Try common passwords:

- (blank)
    
- root
    
- password
    
- mysql
    
- toor
    
- Reused creds from other services
    

---

# 3️ Basic Navigation Commands

show databases;  
use <database>;  
show tables;  
show columns from <table>;  
select * from <table>;  
select version();

Search inside table:

select * from <table> where <column>="admin";

---

# 4️ High-Value Targets

Important databases:

|Database|Why|
|---|---|
|information_schema|Maps all DB structure|
|mysql|User accounts + hashes|
|Any app DB (wordpress, etc.)|Users + passwords|

Find MySQL users:

use mysql;  
select user, password from user;

Find WordPress users:

use wordpress;  
select * from wp_users;

---

#  Dangerous Misconfigurations

Look for:

- Root with empty password
    
- DB exposed to internet
    
- Plain-text creds in config files
    
- `secure_file_priv` not set (file read/write possible)
    
- Verbose debug enabled
    

If file read is possible:

cat /etc/mysql/mysql.conf.d/mysqld.cnf

---

#  Full Attack Summary

1. Scan port 3306
    
2. Try root with no password
    
3. Try weak/default passwords
    
4. Login → show databases
    
5. Find app database
    
6. Dump user table
    
7. Crack hashes (Hashcat/John)
    
8. Reuse credentials on:
    
    - Website login
        
    - SSH
        
    - Other services
        

---

#  Important Notes

- Most websites use MySQL in LAMP/LEMP stacks
    
- Password hashes can be cracked offline
    
- WordPress hashes are often crackable
    
- `information_schema` maps the entire DB
    
- If `secure_file_priv` is empty → OS file read may be possible
    
- Credential reuse is VERY common
    

---

Related Notes:  
[[SNMP Enumeration]]  
[[IMAP & POP3 Enumeration]]  
[[SQL Injection Fundamentals]]  
[[Password Cracking with Hashcat]]

---