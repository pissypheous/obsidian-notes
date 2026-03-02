

**Tags:** #oracle #tns #enumeration #database #pentesting

---

# 1. What Is TNS?

**TNS (Transparent Network Substrate)** = Oracle’s communication protocol.  
Default port: **TCP 1521**

Important files:

- `tnsnames.ora` → Client-side database mappings
    
- `listener.ora` → Server-side listener configuration
    

You need the **SID** (database name) to connect.

---

# 2. Attack Flow (Step-by-Step)

## Step 1 — Confirm Service

sudo nmap -p 1521 -sV <IP> --open

Goal:

- Is port 1521 open?
    
- What Oracle version?
    

---

## Step 2 — Discover the SID

SID = database instance name (required to log in)

sudo nmap -p 1521 --script oracle-sid-brute <IP>

If SID is found → move forward.

---

## Step 3 — Test Default Credentials

Common defaults:

|Username|Password|
|---|---|
|scott|tiger|
|dbsnmp|dbsnmp|
|any (Oracle 9)|CHANGE_ON_INSTALL|

---

## Step 4 — Brute Force Accounts (Automated)

Tool: **ODAT (Oracle Database Attacking Tool)**

./odat.py all -s <IP>

Goal:

- Find valid credentials
    
- Identify misconfigurations
    
- Check privilege level
    

---

## Step 5 — Log In Manually

sqlplus <user>/<pass>@<IP>/<SID>

If successful → enumerate database.

---

## Step 6 — Basic Enumeration (Inside SQL)

List tables:

select table_name from all_tables;

Check roles:

select * from user_role_privs;

Goal:

- Identify sensitive tables
    
- Check if user has admin privileges
    

---

## Step 7 — SYSDBA Privilege Escalation (If Allowed)

sqlplus <user>/<pass>@<IP>/<SID> as sysdba

If it works → full database control.

---

## Step 8 — Dump Password Hashes (High Priv Only)

select name, password from sys.user$;

Then crack offline.

---

## Step 9 — File Upload (Post-Exploitation)

Using ODAT:

./odat.py utlfile -s <IP> -d <SID> -U <user> -P <pass> --putFile <target_path> file.txt ./file.txt

Common web roots:

Linux:

/var/www/html

Windows:

C:\inetpub\wwwroot

Verify:

curl http://<IP>/file.txt

---

# 3. Tool Summary

|Tool|Purpose|
|---|---|
|nmap|Detect service + brute SID|
|odat|Automate enum + exploitation|
|sqlplus|Manual interaction|

---

# Quick Mental Model

1. Is 1521 open?
    
2. Find SID.
    
3. Try default creds.
    
4. Brute force with ODAT.
    
5. Log in.
    
6. Check privileges.
    
7. Escalate if possible.
    
8. Extract data / upload file.
    

---

