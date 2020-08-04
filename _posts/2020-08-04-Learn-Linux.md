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

#### 1-SSH Into the server
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

#### 1-How would you output hello without a newline ?
```bash
echo -n hello
```

##Section 3: Basic File Operations

### -ls 
ls - list directory contents
#### 1-What flag outputs all entries ?
```bash
-a
```

#### 2 What flag outputs things in a "long list" format ?   
```bash
-l
```

### -cat
cat - concatenate files and print on the standard output
#### 1-What flag numbers all output lines ?  
```bash
-n
```


