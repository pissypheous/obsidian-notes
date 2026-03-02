

**Tags:** #ipmi #bmc #enumeration #pentesting

---

# 1. What Is IPMI?

**IPMI (Intelligent Platform Management Interface)** = hardware-level remote management.

Runs on a **BMC (Baseboard Management Controller)**.  
Works even if the OS is:

- Powered off
    
- Crashed
    
- Not installed
    

If you access IPMI → you basically have **physical-level control**.

Default port:

UDP 623

---

# 2. Common BMC Implementations

|Vendor|Product|
|---|---|
|Dell|iDRAC|
|HP|iLO|
|Supermicro|IPMI|

---

# 3. Default Credentials to Try

|Product|Username|Password|
|---|---|---|
|Dell iDRAC|root|calvin|
|HP iLO|Administrator|random 8-char (factory)|
|Supermicro|ADMIN|ADMIN|

Always try:

- Web console
    
- SSH
    
- Telnet
    

Admins often forget to change defaults.

---

# 4. Attack Flow (Step-by-Step)

## Step 1 — Confirm IPMI

sudo nmap -sU -p 623 --script ipmi-version IP

Or Metasploit:

use auxiliary/scanner/ipmi/ipmi_version  
set rhosts IP  
run

Goal:

- Is IPMI running?
    
- What version?
    

---

## Step 2 — Try Default Logins

Attempt:

- Web interface
    
- SSH
    
- Telnet
    

If login works → full remote management access.

---

## Step 3 — Dump Password Hashes (IPMI 2.0 Weakness)

Important:

IPMI 2.0 (RAKP protocol) leaks a password hash **before authentication**.

You do NOT need valid credentials.

Metasploit:

use auxiliary/scanner/ipmi/ipmi_dumphashes  
set rhosts IP  
run

This grabs hashes for valid users.

---

## Step 4 — Crack Hashes Offline

hashcat -m 7300 ipmi.txt wordlist.txt

HP iLO default pattern (8 chars, uppercase + digits):

hashcat -m 7300 ipmi.txt -a 3 ?1?1?1?1?1?1?1?1 -1 ?d?u

---

## Step 5 — Log In & Pivot

After cracking:

- Log into BMC
    
- Check for password reuse
    
- Try credentials on:
    
    - SSH
        
    - Domain accounts
        
    - Other servers
        

One cracked IPMI hash often leads to full network compromise.

---

# 5. Why IPMI Is Dangerous

- Runs outside the OS
    
- Often exposed internally
    
- Default passwords common
    
- IPMI 2.0 hash leak is a **protocol flaw**, not a misconfiguration
    
- Cannot be patched away
    
- Defense = strong passwords + network segmentation
    

---

# Quick Mental Model

1. Is UDP 623 open?
    
2. Identify version.
    
3. Try default creds.
    
4. Dump hashes (no auth needed).
    
5. Crack offline.
    
6. Log in.
    
7. Pivot across network.