Q.### Brute-force vhosts on the target system. What is the full subdomain that is prefixed with "web"? Answer using the full domain, e.g. "x.inlanefreight.htb"
1.ran this command gave me all of the solutions
gobuster vhost -u http://154.57.164.64:32248   -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-20000.txt   --append-domain   --domain inlanefreight.htb   -t 50
