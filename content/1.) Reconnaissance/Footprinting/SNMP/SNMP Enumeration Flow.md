
Tags: #enumeration #snmp #network #pentesting  
Topic: Network Service Enumeration

---

#  Quick Concept

SNMP = protocol to monitor/manage network devices.

Runs on:

- **UDP 161** → Queries
    
- **UDP 162** → Traps (alerts)
    

Versions:

| Version | Secure? | Notes            |
| ------- | ------- | ---------------- |
| v1      | no      | Plain text       |
| v2c     | no      | Plain text       |
| v3      | yes     | Encrypted + auth |

 Focus on **v1 & v2c** (no encryption).

---

#  Community Strings

Community string = SNMP password.

Common defaults:

- `public` → Read-only
    
- `private` → Read/Write
    

 Sent in plain text. Easy to intercept or brute-force.

---

#  What SNMP Can Reveal

If misconfigured, you can get:

- OS + version
    
- Hostname
    
- Contact email
    
- Installed software
    
- Running processes
    
- Network interfaces
    
- System uptime
    

It can leak a LOT.

---

#  Attack Flow

Scan → Find community string → Dump data → Analyze

---

# 1️ Scan for SNMP (UDP)

sudo nmap -sU -p161,162 --open <TARGET_IP>

With script:

sudo nmap -sU -p161 -sV --script snmp-info <TARGET_IP>

Look for:

- UDP 161 open
    
- SNMP version
    
- Any leaked info
    

 Always use `-sU` (UDP scan).

---

# 2️ Try Default Community String

Try:

- `public`
    
- `private`
    

If it works → move to dump phase.

---

# 3️ Brute-Force Community String (onesixtyone)

onesixtyone -c wordlist.txt <TARGET_IP>

If found:

[public]

Now use it with snmpwalk.

---

# 4️ Dump All Data (snmpwalk)

snmpwalk -v2c -c public <TARGET_IP>

Common useful OIDs:

|OID|Info|
|---|---|
|1.3.6.1.2.1.1|System info|
|1.3.6.1.2.1.1.4.0|Contact email|
|1.3.6.1.2.1.1.5.0|Hostname|
|1.3.6.1.2.1.25.4.2|Running processes|
|1.3.6.1.2.1.25.6.3|Installed software|

Look for:

- Emails
    
- OS version
    
- Software versions
    
- Internal hostnames
    

---

#  Faster Enumeration (braa)

Use if snmpwalk is slow:

braa public@<TARGET_IP>:.1.3.6.*

Faster bulk OID scanning.

---

#  Dangerous Misconfigurations

Look for:

- `rocommunity public`
    
- `rwcommunity public`
    
- `rwuser noauth`
    
- SNMPv1/v2c enabled
    

If `rwcommunity` is exposed:  
→ You may be able to change device settings.

---

#  Tool Summary

|Tool|Purpose|
|---|---|
|nmap|Find SNMP port|
|onesixtyone|Brute-force string|
|snmpwalk|Dump data|
|braa|Fast OID scan|

---

#  Full Attack Summary

1. Scan UDP 161
    
2. Try `public`
    
3. Brute-force if needed
    
4. Dump with snmpwalk
    
5. Analyze:
    
    - Emails
        
    - Hostnames
        
    - OS version
        
    - Installed packages
        
    - Running processes
        

---

#  Important Notes

- SNMP = UDP (don’t forget -sU)
    
- Default `public` works often
    
- v3 is harder to attack
    
- Emails found → good for OSINT/phishing
    
- Installed software → find vulnerabilities
    
- Read/write access = high impact
    

---

If you want, I can also create a **1-page ultra-fast exam review version** like a cheat sheet.