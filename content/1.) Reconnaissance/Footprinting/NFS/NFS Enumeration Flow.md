# Enumeration Phase — Quick Checklist

Goal: Identify attack surface → find entry point → exploit

Rule: Enumerate → Think → Enumerate again

---

# 1️⃣ Network Discovery (Always First)

## Full TCP Scan
nmap -Pn -p- --open -T4 <IP>

## Service + Version
nmap -sV -sC -O -p <ports> <IP>

## Optional
nmap -A -T4 <IP>
nmap -sU --top-ports 100 <IP>

→ Identify:
- Web
- SMB
- SSH
- FTP
- DB
- SNMP
- DNS

---

# 2️⃣ Web Enumeration (If HTTP/HTTPS)

## Directories
gobuster dir -u http://target -w /usr/share/wordlists/dirb/common.txt -x php,txt,html

## Subdomains
gobuster dns -d domain.com -w subdomains.txt

## Fingerprint
whatweb http://target
nikto -h http://target

## Manual
- robots.txt
- sitemap.xml
- login pages
- backups
- .git / .env

## CMS
wpscan
joomscan

---

# 3️⃣ SMB Enumeration (If 139/445)

smbclient -L //target -N
smbmap -H target
enum4linux -a target
rpcclient -U "" -N target

Look for:
- Anonymous access
- Credentials
- Scripts
- Config files

---

# 4️⃣ SNMP (If 161)

snmpwalk -v2c -c public <IP>

Try:
public / private / manager

---

# 5️⃣ DNS (If 53)

dig axfr @<IP> domain.com
dig +short ANY domain.com

---

# 6️⃣ Local Enumeration (After Foothold)

## System
whoami
id
hostname
ip a
uname -a
cat /etc/os-release

## Users
cat /etc/passwd | grep bash

## Priv Esc
sudo -l
find / -perm -u=s -type f 2>/dev/null
ls -la /etc/cron*
crontab -l

## Processes / Ports
ps aux
netstat -tuln
ss -tuln

---

# 7️⃣ Quick File Hunting

ls -la /home/*
ls -la /var/www/*
ls -la /tmp/*

---

# 8️⃣ Methodology Loop 🧠

Scan → Enum Service → Exploit → Enum Again

If stuck:
→ Try another service
→ Re-scan
→ Manual testing

Never assume.

---

# Notes

Create host note:
[[Host-10.10.10.10]]

Tag findings:
#creds #rce #lfi #privesc #misconfig #lowhanging

Document:
- Commands
- Output
- Screenshots

---

Last Updated: {{date}}