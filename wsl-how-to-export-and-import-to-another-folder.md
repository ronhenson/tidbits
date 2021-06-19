---
title: "WSL how to export and import to another folder"
created: 20210613185716
tags: [ devops ]
---

links
: [[placeholder]]

# WSL how to export and import to another folder

{} - As of 20-03-24 my wsl files are located: [D:\wsl](D:/wsl) }

```powershell
 wsl --export Ubuntu-20.04 .\Ubuntu-20.04_20-03-24.tar.gz
```

# Import a copy

```powershell
wsl --import ubuntu-2  .\ubuntu-2 .\Ubuntu-20.04_20-03-24.tar.gz --version 2
```
