Tags: #enumeration #mssql #database #windows #pentesting  
Topic: Network Service Enumeration

---

#  Quick Concept

MSSQL = Microsoft’s database system.

- Default Port → **TCP 1433**
    
- Default Admin → `sa`
    
- Auth → Windows Auth (AD) or SQL Auth
    
- Runs on → Windows
    

 Important:  
MSSQL connects to **Active Directory**.  
Compromise can lead to **domain-wide access**.

---

#  What You Can Get

If access is gained:

- Usernames + password hashes
    
- Windows domain info
    
- Saved credentials (SSMS)
    
- Privilege escalation paths
    
- OS command execution (`xp_cmdshell`)
    

---

#  Attack Flow

Scan → Fingerprint → Try `sa` → Connect → Dump DB → Check OS access

---

# 1️ Scan Port 1433

sudo nmap -sV -p1433 --script ms-sql-* <TARGET_IP>

Look for:

- `ms-sql-empty-password`
    
- Version info
    
- Hostname + domain (NTLM info)
    
- Named pipes
    
- Dumped hashes
    

 Even without creds, you can leak:

- Windows hostname
    
- Domain name
    
- SQL version
    

---

# 2️ Quick Fingerprint (Optional)

Metasploit:

use auxiliary/scanner/mssql/mssql_ping  
set RHOSTS <TARGET_IP>  
run

Confirms:

- Server name
    
- Instance name
    
- Version
    
- Port
    

---

# 3️ Try Connecting

Using Impacket:

SQL Auth:

python3 mssqlclient.py sa@<TARGET_IP>

Windows Auth:

python3 mssqlclient.py DOMAIN/user@<TARGET_IP> -windows-auth

Try:

- Blank password
    
- Weak passwords
    
- Reused credentials
    

---

# 4️ Basic Exploration Commands

List databases:

select name from sys.databases;

Switch DB:

use <database>;

List tables:

select * from information_schema.tables;

Dump table:

select * from <table>;

Check permissions:

select system_user;  
select is_srvrolemember('sysadmin');

---

#  High-Value Databases

|Database|Why Check|
|---|---|
|master|Logins + configs|
|msdb|Scheduled jobs|
|Non-default DBs|App data (users + passwords)|

Focus on **non-default databases**.

---

#  xp_cmdshell (OS Command Execution)

Check if enabled:

select * from sys.configurations where name='xp_cmdshell';

Run command:

EXEC xp_cmdshell 'whoami';

 If enabled + sysadmin:  
→ Direct command execution on Windows server.

---

#  Dangerous Misconfigurations

Look for:

- `sa` with blank/weak password
    
- No encryption
    
- Named pipes enabled
    
- `xp_cmdshell` enabled
    
- SSMS installed (saved creds)
    

If `xp_cmdshell` works → full server compromise possible.

---

#  Full Attack Summary

1. Scan port 1433
    
2. Grab hostname + domain info
    
3. Try `sa` with blank password
    
4. Connect with mssqlclient.py
    
5. List databases
    
6. Find app database
    
7. Dump user tables
    
8. Check `xp_cmdshell`
    
9. If enabled → run OS commands
    
10. Use Windows creds for lateral movement
    

---

#  Important Notes

- MSSQL = usually inside AD environments
    
- `sa` blank password is common in weak setups
    
- Named pipes useful if 1433 blocked
    
- Hashes can be cracked (Hashcat) or used for Pass-the-Hash
    
- DB compromise can lead to full domain compromise
    

---

Related Notes:  
[[MySQL Enumeration]]  
[[SNMP Enumeration]]  
[[Active Directory Attacks]]  
[[Lateral Movement Windows]]