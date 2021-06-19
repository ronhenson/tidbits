---
title: "WSL how to shrink the ext4 vhdx file"
created: 20210613185222
tags: [ devops ]
---

links
: [[placeholder]]

# WSL how to shrink the ext4 vhdx file

# **Make sure the wsl is not running**

## Should backup before starting

```powershell
wsl.exe --list --verbose  	#  ffind out what is running
wsl.exe --terminate <name>	# will stop if needed
```


```powershell
diskpart.exe
DISKPART> select vdisk file="D:\wsl\ubuntu-2\ext4.vhdx"
DISKPART> compact vdisk
	100 percent completed
Diskpart succesfully compacted the virtual disk file.
DISKPART>exit
```
