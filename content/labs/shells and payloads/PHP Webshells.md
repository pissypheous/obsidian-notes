Goal: establish a webshell attacking the rConfig web app

1.log into the rConfig server
	a.open firefox copy the attack box ip and paste it in the url bar
	b.the credentials are admin:admin
2.add a vendor
	a.devices>vendors>add vendor
3.download whitewinter wolf php web shell
	a.go into the github repo for white wolf
	b.download the zip file
	c.unzip it and put it in the home directory
4.start burpsuite
	a.open a temp project
5.change network settings in firefox
	a.in the settings scroll down all the way to network
	b.select manual proxy config> 127.0.0.0 all of em>port 8080 all of em
6.add the whitewolf webshell in firefox
	a.click save
	b.notice in burpsuite is hanging as if its stuck loading>hit the forward in burpsuite
7.change the content type in burpsuite
	a.change the content type option from application/x -php to image/gif hit the forward button twice
8.turn interceptor off in burpsuite
	a.turn interceptor off in burpsuite
	b.go back into the browser to see the results
	c.the message added new vendor lest you know the file upload was succesful
9.navigate to the webshell
	a.put the ip of the attack box/images/vendor/webshell.php
10.in the webshell navigate to the selected folder
	a.note under cwd you are in /home/rConfig/www/images/vendor
	b.run ls under the cmd to print out the answer