**Tags:** [#smb](app://obsidian.md/index.html#smb) [#enumeration](app://obsidian.md/index.html#enumeration) [#footprinting](app://obsidian.md/index.html#footprinting)

## Overview

SMB (Server Message Block) = file sharing, printers, network resources.

**Ports**

- 139 → NetBIOS SMB (legacy)  
    
- 445 → Direct SMB (modern)  
    

---

# Phase 1 — Port Scan

sudo nmap -sV -sC -p 139,445 <TARGET_IP>

**Look For**

- SMB version (v1 = vulnerable)  
    
- Message signing (disabled = relay risk)  
    
- Hostname / domain info  
    

---

# Phase 2 — Anonymous Enumeration (Null Session)

List shares:

smbclient -N -L //<TARGET_IP>

Permissions:

smbmap -H <TARGET_IP>

CrackMapExec:

crackmapexec smb <TARGET_IP> --shares -u '' -p ''

**Look For**

- READ / WRITE access  
    
- Non-default shares  
    
- Guest access enabled  
    

---

# Phase 3 — Connect to Shares

smbclient //<TARGET_IP>/

Common commands:

ls  
cd

  
get  
mget *

Local shell:

!ls  
!cat

Goal → Download interesting files (configs, creds, backups).

---

# Phase 4 — RPC Enumeration (Users & System Info)

rpcclient -U "" <TARGET_IP>

Useful commands:

srvinfo  
enumdomains  
querydominfo  
netshareenumall  
enumdomusers  
queryuser

Goal → Usernames, domain info, shares.

---

# Phase 5 — RID Bruteforce (If Users Hidden)

for i in (printf '%x\n' $i)"  
done

Why → Some servers block listing but allow RID lookup.

---

# Phase 6 — Automated Enumeration

### enum4linux-ng (Best Overall)

./enum4linux-ng.py <TARGET_IP> -A

### Impacket SAMR Dump

samrdump.py <TARGET_IP>

**Look For**

- Usernames  
    
- Password policy  
    
- Domain info  
    

---

# Common Misconfigurations (Samba)

|Setting|Risk|
|---|---|
|guest ok = yes|Anonymous access|
|browseable = yes|Visible shares|
|writable = yes / read only = no|File modification|
|create mask = 0777|World writable|
|map to guest = bad user|Auth bypass|

---

# SMB Versions (Quick Reference)

|Version|Risk|
|---|---|
|SMBv1|EternalBlue (MS17-010)|
|SMBv2/3|Modern|

---

# Attack Opportunities

✔ Null session → enumerate everything  
✔ READ share → download data  
✔ WRITE share → upload payload  
✔ Usernames → password spraying  
✔ SMBv1 → EternalBlue  
✔ Signing disabled → NTLM relay  
✔ Weak policy → brute force

---

# Quick Pentest Workflow

1. Scan ports
2. Check anonymous access
3. Enumerate shares
4. Dump users
5. Download files
6. Try credentials / exploits