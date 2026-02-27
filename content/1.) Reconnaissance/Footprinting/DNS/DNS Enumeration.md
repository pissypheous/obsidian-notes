

Tags: #dns #enumeration #footprinting #pentesting #network

---

##  Objective

Identify:

- Nameservers
    
- Public records
    
- Internal hosts
    
- Zone transfer misconfigs
    
- Hidden subdomains
    
- DNS software version
    

---

#  Phase 1 — Identify Nameservers

>  Goal: Find authoritative DNS servers

dig ns <domain> @<dns-ip>

###  Look For

- NS records
    
- Additional A records (IP of NS)
    
- Multiple nameservers (try each)
    

---

#  Phase 2 — Enumerate Public Records

>  Goal: Extract visible DNS data

dig any <domain> @<dns-ip>

###  Analyze

- **A / AAAA** → IP mapping
    
- **MX** → Mail servers
    
- **TXT** → SPF, verification, cloud usage
    
- **SOA** → Admin email, serial
    

---

#  Phase 3 — Attempt Zone Transfer (AXFR)

>  High Value Step

dig axfr <domain> @<dns-ip>

Also test:

dig axfr internal.<domain> @<dns-ip>

###  If Successful

You get:

- All subdomains
    
- Internal IP ranges
    
- VPN / DC hosts
    
- Dev / staging systems
    

###  Why It Works

Misconfiguration:

allow-transfer any;

---

#  Phase 4 — Subdomain Discovery

>  Goal: Find hidden assets

---

###  Quick Wordlist Method

for sub in $(cat wordlist.txt); do  
  dig $sub.<domain> @<dns-ip> +short  
done

---

###  Automated

dnsenum --dnsserver <dns-ip> -f wordlist.txt <domain>

---

###  Look For

- dev.
    
- admin.
    
- vpn.
    
- staging.
    
- internal.
    
- test.
    

These are often weak targets.

---

#  Phase 5 — Reverse Lookup (PTR)

>  Map IP → Hostname

dig -x <ip>

Useful after discovering IP ranges.

---

#  Phase 6 — DNS Version Fingerprinting

>  Identify DNS software

dig CH TXT version.bind @<dns-ip>

###  Why

- Identify BIND / Windows DNS
    
- Check CVEs
    
- Plan exploits
    

---

#  Record Quick Reference

|Record|Meaning|Why It Matters|
|---|---|---|
|A|IPv4|Map host → IP|
|AAAA|IPv6|IPv6 attack surface|
|NS|Nameserver|AXFR target|
|MX|Mail|Phishing recon|
|TXT|Text|Cloud leaks|
|CNAME|Alias|Subdomain takeover|
|PTR|Reverse lookup|Network mapping|
|SOA|Authority|Admin email|
|AXFR|Zone transfer|Full zone dump|

---

#  Misconfigurations to Check

- `allow-transfer any;`
    
- `allow-query any;`
    
- `allow-recursion any;`
    
- Version disclosure enabled
    

---

#  Tools

- `dig` → Manual queries
    
- `dnsenum` → Brute force
    
- `amass` → Advanced enum
    
- `subfinder` → Passive recon
    
- `fierce` → DNS recon
    

---

#  Engagement Logging Template

## DNS Findings  
  
Target:  
DNS Server IP:  
Nameservers:  
Zone Transfer: Yes / No  
Subdomains Found:  
Internal IPs:  
Mail Servers:  
Interesting TXT Records:  
DNS Version:
