1.The Goal: Connecting to Windows with `xfreerdp` and Mounting a Local Folder

1.Create a folder on your Pwnbox to share:
	a.mkdir ~/transfer
2.Connect via xfreerdp and mount that folder:
	a.xfreerdp /v:10.129.21.3 /u:htb-student /p:'HTB_@cademy_stdnt!' /drive:linux,/home/htb-student/transfer
3.Practice uploading (Pwnbox → Target):
	a.Put a file into `~/transfer` on your Pwnbox
	b.It instantly appears in `\\tsclient\linux` on the Windows machine
	c.Copy it somewhere on the Windows machine (like the Desktop)
4.Practice downloading (Target → Pwnbox):
	a.Copy any file from the Windows machine into `\\tsclient\linux`
	b.Check your `~/transfer` folder on Pwnbox — it'll be there