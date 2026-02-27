# Network Discovery (Always First)

## Full TCP Scan

nmap -Pn -p- --open -T4 <IP>

## Service + Version Detection

nmap -sV -sC -O -p <ports> <IP>

## Optional Scans

nmap -A -T4 <IP>  
nmap -sU --top-ports 100 <IP>

### Identify Services

- Web
    
- SMB
    
- SSH
    
- FTP
    
- Database
    
- SNMP
    
- DNS
    

---

# 2. Web Enumeration (If HTTP/HTTPS)

## Directory Brute Force

gobuster dir -u http://target -w /usr/share/wordlists/dirb/common.txt -x php,txt,html

## Subdomain Brute Force

gobuster dns -d domain.com -w subdomains.txt

## Fingerprinting

whatweb http://target  
nikto -h http://target

## Manual Checks

- robots.txt
    
- sitemap.xml
    
- Login pages
    
- Backup files
    
- `.git`
    
- `.env`
    

## CMS Enumeration

wpscan  
joomscan

---

# 3. SMB Enumeration (If Ports 139/445 Open)

smbclient -L //target -N  
smbmap -H target  
enum4linux -a target  
rpcclient -U "" -N target

### Look For

- Anonymous access
    
- Credentials
    
- Scripts
    
- Config files
    

---

# 4. SNMP Enumeration (If Port 161 Open)

snmpwalk -v2c -c public <IP>

### Try Community Strings

- public
    
- private
    
- manager
    

---

# 5. DNS Enumeration (If Port 53 Open)

dig axfr @<IP> domain.com  
dig +short ANY domain.com

---

# 6. Local Enumeration (After Foothold)

## System Information

whoami  
id  
hostname  
ip a  
uname -a  
cat /etc/os-release

## Users

cat /etc/passwd | grep bash

## Privilege Escalation Checks

sudo -l  
find / -perm -u=s -type f 2>/dev/null  
ls -la /etc/cron*  
crontab -l

## Processes and Open Ports

ps aux  
netstat -tuln  
ss -tuln

---

# 7. Quick File Hunting

ls -la /home/*  
ls -la /var/www/*  
ls -la /tmp/*

---

# 8. Methodology Loop

Scan → Enumerate service → Exploit → Enumerate again

If stuck:

- Try another service
    
- Re-scan
    
- Perform manual testing
    

Never assume.

---

# Notes

## Create Host Note

[[Host-10.10.10.10]]

## Tag Findings

#creds #rce #lfi #privesc #misconfig #lowhanging

## Document Everything

- Commands used
    
- Output
    
- Screenshots