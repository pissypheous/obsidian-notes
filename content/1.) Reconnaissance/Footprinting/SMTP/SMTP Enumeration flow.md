


SMTP Enumeration & Exploitation Flow
Tags: #smtp #enumeration #relay #vrfy #email #pentest

Goal: Identify Users → Check Relay → Assess Abuse Potential

Ports:
- 25 (SMTP)
- 465 (SMTPS)
- 587 (Submission)



# Phase 1 — Initial Recon (Always First)

##  Version + Default Scripts
```bash
nmap -sV -sC -p25,465,587 <TARGET>
````

### Look For:

- Mail server type (Postfix / Exim / Sendmail)
    
- Version number
    
- Banner info (`smtpd_banner`)
    
- TLS support
    

##  SMTP-Specific Scripts

```bash
nmap -p25 --script smtp-commands,smtp-open-relay,smtp-enum-users <TARGET> -v
```

What They Do:

- `smtp-commands` → lists supported commands (EHLO reply)
    
- `smtp-open-relay` → tests relay misconfig
    
- `smtp-enum-users` → attempts user enumeration
    

---

# Phase 2 — Manual Interaction (Important)

## Connect

```bash
telnet <TARGET> 25
# or
nc <TARGET> 25
```

## Start Session Properly

```
EHLO test.local
# fallback:
HELO test.local
```

### Good Response Indicators

```
250-STARTTLS        → encryption available
250-AUTH PLAIN LOGIN → authentication enabled
250-PIPELINING
250-SIZE
```

Document supported extensions.

---

# Phase 3 — User Enumeration (Main Objective)

Goal: Find valid usernames.

## Technique A — VRFY (Often Blocked)

```
VRFY root
VRFY admin
VRFY administrator
```

Response Meaning:

- `250` → likely exists
    
- `550` → does not exist
    
- `252` → ambiguous / server lying
    

Low reliability on modern servers.

---

## Technique B — EXPN (Alias Enumeration)

```
EXPN support
EXPN abuse
```

May leak:

- Mailing lists
    
- Aliases
    
- Internal usernames
    

---

## Technique C — RCPT TO (Most Reliable)

```
MAIL FROM:<test@domain.com>
RCPT TO:<admin@target.com>
```

Response Meaning:

- `250 2.1.5 Ok` → mailbox exists
    
- `550 5.1.1` → mailbox does not exist
    

Most consistent method in modern environments.

Use wordlists:

```
/usr/share/seclists/Usernames/top-usernames-shortlist.txt
```

---

# Phase 4 — Open Relay Testing (High Severity Finding)

## Manual Relay Test

```
MAIL FROM:<test@external.com>
RCPT TO:<external@nmap.scanme.org>
DATA
Subject: test
relay test
.
```

If response:

```
250 2.0.0 Ok: queued
```

→ Open relay confirmed.

Always verify manually even if Nmap detects it.

---

## Misconfiguration Indicators (Postfix Example)

```
mynetworks = 0.0.0.0/0        ← critical
mynetworks = 10.0.0.0/8       ← too broad
```

Improper relay restrictions = major finding.



# Phase 5 — Abuse Opportunities

|Finding|Impact|
|---|---|
|Valid usernames|Phishing targets / password spray|
|Open relay|Internal phishing / data exfil|
|STARTTLS optional|Possible downgrade attack|
|Old mail version|Check CVEs / RCE potential|
|Weak auth methods|Brute force possible|



# Quick Pentest Checklist

-  Nmap -sV -sC
    
-  Run smtp-* scripts
    
-  Manual EHLO → record extensions
    
-  Try VRFY / EXPN
    
-  Perform RCPT TO enumeration
    
-  Test open relay manually
    
-  Check TLS (STARTTLS / 465 / 587)
    
-  Document banner + config clues
    



# Attack Flow Summary

Usernames Found → Password Spray / Phishing  
Open Relay → Internal Abuse  
Weak Config → Possible RCE / DoS  
TLS Optional → Downgrade Risk



Related:  
[[SMTP]]  
[[Phishing]]  
[[Misconfiguration]]  
[[Information Disclosure]]  
[[Password Spraying]]
