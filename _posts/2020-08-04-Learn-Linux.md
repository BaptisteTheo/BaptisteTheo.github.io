---
layout: post
title: TryHackme WalkThrough Learn Linux 
author: Baptiste
date: 2020-08-04
categories: [try-hack-me, walk-through]
tags: [try-hack-me, linux, easy]
math: true
---

# Learn Linux

A guided room designed to teach you the linux basics

## Section 1 SSH 

SSH allows you to run commands interactively on the remote machine. 
This is done through the use of a program on the target machine, which allows the ssh client to interface with the target host.
While the most common usage of a regular operating system is graphical(allowing you to see pictures, web browsers, file managers etc.) SSH works through a command line, meaning anything done on the target machine will be donethrough a command prompt similar to this.

#### T1-SSH Into the server
- _I put the ip into a variable so I don't need to tap it every time_
```bash 
IP = 'your IP'
```
```bash
ssh shiba1@$IP
password: shiba1
```

## Section 2: Running Commands 

### Basic command Execution

echo - display a line of text.
```bash
echo hello
```

### Manual Pages and Flags
man - an interface to the on-line reference manuals
if not man, use the -h flag instead. 

#### T1-How would you output hello without a newline ?
```bash
echo -n hello
```

## Section 3: Basic File Operations

### -ls 
ls - list directory contents
#### T1-What flag outputs all entries ?
```bash
-a
```

#### T2-What flag outputs things in a "long list" format ?   
```bash
-l
```

### -cat
cat - concatenate files and print on the standard output
#### T1-What flag numbers all output lines ?  
```bash
-n
```

### Running a Binary

#### T1-How would you run a binary called hello using the directory shortcut . ?
```bash
./hello
```

#### T2-How would you run a binary called hello in your home directory using the shortcut ~ ?
```bash
~/hello
```

#### T3-How would you run a binary called hello in the previous directory using the shortcut .. ? 
```bash 
../hello
```

## Binary - Shiba1 
```bash
touch noot.txt
./shiba1
```
#### T4-Whats the password for shiba2 ?
pinguftw 

## su

#### T1-How do you specify which shell is used when you login?   
```bash
-s
```

## Section 4: Linux Operators

### ">" 
> is the operator for output redirection. Meaning that you can redirect the output of any command to a file. For example if I were to run echo hello > file, then instead of outputting hello to the console, it would save that output to a file called file.

-_If you user the operator on a file that already exist, the content of the file would be erased_

#### T1-How would you output twenty to a file called test
```bash 
echo twenty > test
```

### ">>" 
>> does mainly the same thing as >, with one key difference. >> appends the output of a command to a file, instead of erasing it.

### "&&" 
Stand for "and". Allow you to execute a second command, only if the first executed successfull

### "&"
& is a background operator. Allow you to run directly a command that would normally take time, normally you wouldn't be able
to run commands during that period, with & you can run other command. 

### "$"
The $ is unusuall and user . These are variables set by the computer, or set them yourself, like I did with the $IP.
If you edit these variables you can change certain processes on your computer. 
```bash
shiba2@nootnoot:/home/shiba1$ echo $USER
shiba2
shiba2@nootnoot:/home/shiba1$
```
You can set a variable with "="
Or a environnement variables with 
```bash 
export <varname>==<value>
```

#### T1-How would you set nootnoot equal to 1111 ?
```bash
export nootnoot=1111
```

#### T2-What is the value of the home environment variable ?
```bash
shiba2@nootnoot:/home/shiba1$ echo $HOME
/home/shiba2
```

### "|"
| Stand for pipe, It's a unique operators that allows you to take the output of a command and use it as input 
for a second command.
