

Tags: #enumeration #email #imap #pop3 #pentesting  
Topic: Network Service Enumeration

---

#  Quick Concept

|Protocol|Plain Port|SSL Port|Purpose|
|---|---|---|---|
|IMAP|143|993|Sync + manage mail on server|
|POP3|110|995|Download mail (often removes from server)|

**Main Difference**

- IMAP = stays on server (syncs devices)
    
- POP3 = downloads mail locally
    

---

#  Attack Flow

Scan → Grab Info → Test Login → Read Mail

---

# 1️ Scan Ports

sudo nmap <TARGET_IP> -sV -p110,143,993,995 -sC

Look for:

- Service version (ex: Dovecot)
    
- SSL certificate info (hostnames, emails, org)
    
- Supported capabilities
    

SSL certs often leak:

- Internal hostnames
    
- Employee emails
    
- Organization name
    

---

# 2️ Test Credentials (IMAP SSL)

curl -k 'imaps://<TARGET_IP>' --user username:password

Verbose mode:

curl -k 'imaps://<TARGET_IP>' --user username:password -v

Look for:

- Folder names (INBOX, Sent, etc.)
    
- Server banner/version
    
- TLS details
    

---

# 3️ Connect Manually (SSL)

IMAP:

openssl s_client -connect <TARGET_IP>:993

POP3:

openssl s_client -connect <TARGET_IP>:995

After connection, type commands manually.

---

# 4️ IMAP Quick Commands

1 LOGIN user pass  
1 LIST "" *  
1 SELECT INBOX  
1 FETCH 1 all  
1 LOGOUT

---

# 5️ POP3 Quick Commands

USER username  
PASS password  
STAT  
LIST  
RETR 1  
DELE 1  
QUIT

---

#  Misconfigurations to Check

If using Dovecot, look for:

- auth_debug
    
- auth_debug_passwords
    
- auth_verbose
    
- auth_verbose_passwords
    
- auth_anonymous_username
    

These may:

- Log passwords
    
- Allow anonymous login
    
- Reveal why login fails
    

---

#  Easy Wins

Always try:

- username:username
    
- admin:admin
    
- mail:mail
    
- Reused creds from other services
    

---

#  Full Flow Summary

1. Scan ports (110,143,993,995)
    
2. Read SSL cert
    
3. Try credentials
    
4. Connect (curl / openssl)
    
5. Read emails
    
6. Look for:
    
    - Credentials
        
    - Internal info
        
    - Sensitive data
        

---

#  Important Notes

- 110/143 = plaintext (try first)
    
- 993/995 = SSL
    
- SMTP = sending mail
    
- IMAP/POP3 = reading mail
    
- Anonymous login = instant access
    
- Emails often contain passwords for other systems
    
