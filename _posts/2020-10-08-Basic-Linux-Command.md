---
layout: post
title: Linux Basic Commands
author: Baptiste
image: /assets/img/Blue/tux.png
date: 2020-09-20
categories: [Note, Linux, Commands]
tags: [Linux, Note, Basics] 
math: true
--- 

# Linux Basic Commands

This page contains explanation about Linux Commands, this can serve for a fresh reminder or for a person that never used Linux Before.

## Section 1: Running Commands

### echo 
echo - display a line of text.
```bash 
echo hello
```

### man 
man - an interface to the on-line reference manuals.
if man don't work, you can test the -h flag instead.
```bash
man ls
```

## Section 2: File Operations

### ls
ls - list directory contents.
```bash 
ls
```
You can print a long list format with the flag -l
```bash 
ls -l
```

### cat 
cat - concatenate files and print on the standard output
```bash
cat
```

### Running a binary
```bash
./file
```


### Section 4: Linux Operators

### > 
> Is the operator for output redirection. Meaning that you can redirect the output of any command to a file.
For exemple if I were to run echo hello > file, instead of outputting hello to the console, it would save that 
ouput to a file called file.

-_If you use the operator on a file that already exist, the content of the file would be erased_

### >> 
>> Does mainly the same thing as >, with one key difference. >> appends the output of a command to a file, 
instead of erasing it.

### && 
Stand for "and". Allow you to execute a second command, only if the first executed successfully. 

### & 
& is a background operator. Allow you to run direclty a command that would normally take time,
Normally you wouldn't be able to run commands during that period, with & you can run other command.

### $ 
The $ is unusuall. These are variables set by the computer, or set by yourself. 
If you edit these variables you can change certain processes on your computer (if you have the permission...).

Exemple with the variable $USER:
```bash
root@kali:~# echo $USER
root
```
You can set a variable with =
Or a environnement variables with 
```bash
export <varname>==<value>
```
You can concatenate a new variable with an other
```bash
var1={user}Oaken
echo $var1
rootOaken
```

### set 
List the local variable.

### unset 
Cancel the definition of a local variable.

### :-
```bash
echo ${var:-Banane}
```
If the var is empty, put the string "Banane" in it. 

### :=
Affect and execute.
```bash cat file
var=KIWI
echo ${var:=BANANE}
root@kali: ./file
KIWI
```

### $$
Give you the pid of the current processus.

### $! 
Give you the pid in the last processus in the background.

### $#
Give the number of arguments given in the file.

### |
| Stand for pipe, it's a unique operator that allows you to take the output of a command and use it as input 
for a second command.

### kill
Send a signal to a current process.

### ``
Will execute the command, not a string. 

### shift 
Pass to the next arguments.
You can give -n arguments after the shift. 

## Section 3: Advanced File Operations


### chmod 

chmod is for changing permissions for a file, 
You have 3 options, executed, written, read. 
If you use the command ls with the flag -la, you can see the current authorization of each file in the directory
```bash
shiba2@nootnoot:~$ ls -la
total 48
drwxr-xr-x 5 shiba2 shiba2 4096 Aug 19 18:58 .
drwxr-xr-x 8 root   root   4096 Feb 22 21:16 ..
-rw------- 1 shiba2 shiba2 2078 Feb 20 01:59 .bash_history
-rw-r--r-- 1 shiba2 shiba2  220 Feb 13  2020 .bash_logout
-rw-r--r-- 1 shiba2 shiba2 3771 Feb 13  2020 .bashrc
drwx------ 2 shiba2 shiba2 4096 Aug 19 18:58 .cache
drwx------ 3 shiba2 shiba2 4096 Aug 19 18:58 .gnupg
drwxrwxr-x 3 shiba2 shiba2 4096 Feb 19  2020 .local
-rw-r--r-- 1 shiba2 shiba2  807 Feb 13  2020 .profile
-rwsrwxrwx 1 root   root   8472 Feb 20 01:58 shiba2
```
The attributes on the left are in order, the file permissions, owner of the file, and group that the file is in.
The permissions are set with three digit number, each digit controls a specific permission, 
First one: the permissions for a user.
Second one: the permission for a group.
Third one: everyone that's not a part of the user or group.

```bash 
Digit	Meaning
1	That file can be executed
2	That file can be written to
3	That file can be executed and written to
4	That file can be read
5	That file can be read and executed
6	That file can be written to and read
7	That file can be read, written to, and executed
```
A famous one is chmod 777, which give you all the authorisations on a file !

### chown 

chown is a command used to change file owner and group for any file.
The syntax is : 
```bash
chown user:group file
``` 
! You can only use chown if you are above that other user, so you want to be the root user in general.
chown can be also used without specifying a group. 
```bash
chown user file
```

### rm 
rm stands from remove, so use it carefully young padawan.
Remember that you need certain right to remove specific file (configuration file per exemple), so be carefull when you are logged as root. 
If you are a dangerous person you can use the -r flag to deletes every file in a directory.

### mv 
mv stands from move, this command allows you to move files from one place to another.
```bash
mv "file" "destination"
```
You can also change the name of the file with that command. 
```bash
mv "file" "New file name"
```

### cp 
cp stands for copy, instead of moving the file it copies it. 
```bash
cp "file" "destination"
```

### cd 
cd stands for change directory, you can change the location of the current directory with the cd command.
```bash
cd "directory"
```
Relative paths and absolute paths are supported. 

### mkdir
mkdir stands for make directory, this commands allows you to create a new directory or the rename it like the mv command
```bash
mkdir "directory"
```

### find 
The find command allows you to find files (surprise !), It does this by listing every file in the current directory, if we run this command with /etc.
```bash 
find /etc
```
It would list every file in the /etc directory.
You can read the manual page, cause there is a lot of crazy options to use with this command.

### grep 
Before jumping into explination, I HIGHLY recommend you to test this command by yourself to understand how that's working.

grep is maybe the most useful commands to learn. grep allows you to find data inside of data.
When working with large files, or a large output it is arguably the best way to narrow the ouput down to better find what you looking for.
```bash 
grep "string" "file" 
```
For example, you know the name of the file in this case "KsoBaba.ovpn", but don't know where he is, find can be used and grep to find the actual file. 
```bash 
root@kali:~/Downloads# find /* | grep KsoBaba.ovpn
/root/Downloads/KsoBaba.ovpn
```
I found my file ! 
If you don't understand the syntax yet, it's totally normal, like any other command, the more you use it, the more you understand it.

## Section 4 : Other useful commands 

### sudo 
sudo is a command that is serving as Linux's run as administrator button. 
```bash 
sudo "command"
```


