1.
The goal is:

1. Connect to that web server
2. Download the file
3. Open the file
4. Submit the text inside it
5. 5. wget http://10.129.201.55/flag.txt


2.(page 1 on notebook)
The goal is:
	1.download a file to linux
		a.wget right click on the file to get url
	2.Transfer it to Windows
		a.connect to the windows machinexfreerdp /v:TARGET_IP /u:htb-student /p:HTB_@cademy_stdnt!
		b.turn the linux machine into a web server
	3.download the file into windows
		a.iwr http://YOUR_PWNBOX_IP:8080/upload_win.zip -OutFile upload_win.zip
	4.unzip it
		a.Expand-Archive .\upload_win.zip
	5.run the hasher program that the exercise wants you to run 
		a.hasher.exe .\upload_win\upload_win.txt
	