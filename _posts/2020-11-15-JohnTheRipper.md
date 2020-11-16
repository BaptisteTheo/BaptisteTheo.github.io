---
layout: post 
title: John The Ripper 
author: Edwards 
date: 2020-11-15
math: true
--- 

_It is among the most frequently used password testing and breaking programs[3] as it combines a number of password crackers into one package, autodetects password hash types, and includes a customizable cracker. It can be run against various encrypted password formats including several crypt password hash types most commonly found on various Unix versions (based on DES, MD5, or Blowfish), Kerberos AFS, and Windows NT/2000/XP/2003 LM hash. Additional modules have extended its ability to include MD4-based password hashes and passwords stored in LDAP, MySQL, and others._

## Crack RSA key
use ssh3john to change the key to a readible format for John
```bash
ssh2john ./id_rsa > id_rsa.txt
```
Then use john to find the passphrase.
```bash
john --wordlist=/home/edwards/Downloads/rockyou.txt id_rsa.txt
```

```bash
chmod 400 id_rsa
```
_To use it for connecting with SSH_

## Crack hash with etc/passwd and etc/shadow 
```bash 
unshadow passwd.txt shadow.txt > passwords.txt
john --wordlist=/home/edwards/Downloads/rockyou.txt passwords.txt 
john --show passwords.txt 
```

## Crack hash with hashdump result
```bash
john password.txt --wordlist=/home/edwards/Downloads/rockyou.txt --format=NT
```
