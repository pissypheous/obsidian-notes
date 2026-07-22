1.student need to rdp into target machine
	a.xfreerdp /v:STMIP /u:htb-student /p:HTB_@cademy_stdnt!
2.start a privileged `netcat` listener on port 443 on attacker machine
	a.sudo nc -lvnp 443
3.on the Windows target machine, students need to use a PowerShell reverse shell command to connect back to the listener on port 443 in Pwnbox
	a.i cant paste the command here because window defender deletes the file, just google it they all follow the same format
4.After executing the reverse shell user will then receive the reverse shell on their Pwnbox
